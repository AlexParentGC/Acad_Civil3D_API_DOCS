# SampleLine

**Namespace:** `Autodesk.Civil.DatabaseServices`  
**Assembly:** `AeccDbMgd.dll`

## Overview

The `SampleLine` class represents a cross-section cut line used to sample corridor, surface, and pipe network data along an alignment. Sample lines are essential for creating cross-section views and calculating quantities.

**Key Concept:** Sample lines define where cross-sections are taken along an alignment. They can be placed at regular intervals, at specific stations, or at geometric points (PIs, grade breaks, etc.).

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ SampleLine
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets the sample line name |
| `Station` | `double` | Gets the station along the alignment |
| `LeftSwathWidth` | `double` | Gets/sets the left sample width |
| `RightSwathWidth` | `double` | Gets/sets the right sample width |
| `SampleLineGroupId` | `ObjectId` | Gets the parent sample line group |
| `AlignmentId` | `ObjectId` | Gets the associated alignment |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetSectionIds()` | `ObjectIdCollection` | Gets all section view IDs for this sample line |

## Common Usage Patterns

### 1. Listing Sample Lines in a Group

```csharp
using Autodesk.Civil.ApplicationServices;
using Autodesk.Civil.DatabaseServices;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.Geometry;

[CommandMethod("LISTSAMPLELINES")]
public void ListSampleLines()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = acDoc.Editor;
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    
    if (civilDoc == null) return;
    
    using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
    {
        ObjectIdCollection slgIds = civilDoc.GetSampleLineGroupIds();
        
        ed.WriteMessage($"\n=== Sample Line Groups ({slgIds.Count}) ===");
        
        foreach (ObjectId slgId in slgIds)
        {
            SampleLineGroup slg = tr.GetObject(slgId, OpenMode.ForRead) as SampleLineGroup;
            
            ed.WriteMessage($"\n\nGroup: {slg.Name}");
            ed.WriteMessage($"\n  Alignment: {slg.AlignmentId.GetObject(OpenMode.ForRead).Name}");
            
            ObjectIdCollection slIds = slg.GetSampleLineIds();
            ed.WriteMessage($"\n  Sample Lines: {slIds.Count}");
            
            foreach (ObjectId slId in slIds)
            {
                SampleLine sl = tr.GetObject(slId, OpenMode.ForRead) as SampleLine;
                
                ed.WriteMessage($"\n    Station {sl.Station:F2}: {sl.Name}");
                ed.WriteMessage($"\n      Left Width: {sl.LeftSwathWidth:F2}");
                ed.WriteMessage($"\n      Right Width: {sl.RightSwathWidth:F2}");
            }
        }
        
        tr.Commit();
    }
}
```

### 2. Creating Sample Lines at Specific Stations

```csharp
[CommandMethod("CREATESAMPLELINES")]
public void CreateSampleLinesAtStations()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = acDoc.Editor;
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    
    if (civilDoc == null) return;
    
    // Prompt for alignment
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect alignment: ");
    peo.SetRejectMessage("\nMust be an alignment.");
    peo.AddAllowedClass(typeof(Alignment), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
    {
        Alignment alignment = tr.GetObject(per.ObjectId, OpenMode.ForRead) as Alignment;
        
        // Create sample line group
        ObjectId slgId = SampleLineGroup.Create("MySampleGroup", per.ObjectId);
        SampleLineGroup slg = tr.GetObject(slgId, OpenMode.ForWrite) as SampleLineGroup;
        
        // Define stations (every 25m)
        double startStation = alignment.StartingStation;
        double endStation = alignment.EndingStation;
        double interval = 25.0;
        
        for (double station = startStation; station <= endStation; station += interval)
        {
            // Create sample line at this station
            ObjectId slId = slg.AddSampleLineAt($"SL_{station:F0}", station);
            SampleLine sl = tr.GetObject(slId, OpenMode.ForWrite) as SampleLine;
            
            // Set swath widths (50m left and right)
            sl.LeftSwathWidth = 50.0;
            sl.RightSwathWidth = 50.0;
            
            ed.WriteMessage($"\nCreated sample line at station {station:F2}");
        }
        
        tr.Commit();
        ed.WriteMessage($"\n\nCreated {slg.GetSampleLineIds().Count} sample lines");
    }
}
```

### 3. Sampling Surface Elevations

```csharp
[CommandMethod("SAMPLESURFACE")]
public void SampleSurfaceAlongLine()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = acDoc.Editor;
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    
    if (civilDoc == null) return;
    
    // Select sample line
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect sample line: ");
    peo.SetRejectMessage("\nMust be a sample line.");
    peo.AddAllowedClass(typeof(SampleLine), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
    {
        SampleLine sl = tr.GetObject(per.ObjectId, OpenMode.ForRead) as SampleLine;
        SampleLineGroup slg = tr.GetObject(sl.SampleLineGroupId, OpenMode.ForRead) as SampleLineGroup;
        
        ed.WriteMessage($"\n=== Sample Line: {sl.Name} ===");
        ed.WriteMessage($"\nStation: {sl.Station:F2}");
        
        // Get surfaces sampled by this group
        ObjectIdCollection surfaceIds = slg.GetSampledSurfaceIds();
        
        ed.WriteMessage($"\n\nSampled Surfaces ({surfaceIds.Count}):");
        
        foreach (ObjectId surfId in surfaceIds)
        {
            TinSurface surface = tr.GetObject(surfId, OpenMode.ForRead) as TinSurface;
            
            if (surface != null)
            {
                ed.WriteMessage($"\n  Surface: {surface.Name}");
                
                // Get elevation at sample line station
                try
                {
                    Alignment alignment = tr.GetObject(sl.AlignmentId, OpenMode.ForRead) as Alignment;
                    Point3d point = alignment.GetPointAtStation(sl.Station, 0); // Centerline
                    
                    double elevation = surface.FindElevationAtXY(point.X, point.Y);
                    ed.WriteMessage($"\n    Elevation at centerline: {elevation:F3}");
                }
                catch
                {
                    ed.WriteMessage($"\n    (Point outside surface)");
                }
            }
        }
        
        tr.Commit();
    }
}
```

### 4. Modifying Sample Line Widths

```csharp
[CommandMethod("ADJUSTWIDTHS")]
public void AdjustSampleLineWidths()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = acDoc.Editor;
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    
    if (civilDoc == null) return;
    
    // Prompt for new width
    PromptDoubleOptions pdo = new PromptDoubleOptions("\nEnter new swath width (both sides): ");
    pdo.DefaultValue = 50.0;
    pdo.AllowNegative = false;
    pdo.AllowZero = false;
    
    PromptDoubleResult pdr = ed.GetDouble(pdo);
    if (pdr.Status != PromptStatus.OK) return;
    
    double newWidth = pdr.Value;
    
    using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
    {
        ObjectIdCollection slgIds = civilDoc.GetSampleLineGroupIds();
        int totalUpdated = 0;
        
        foreach (ObjectId slgId in slgIds)
        {
            SampleLineGroup slg = tr.GetObject(slgId, OpenMode.ForRead) as SampleLineGroup;
            ObjectIdCollection slIds = slg.GetSampleLineIds();
            
            foreach (ObjectId slId in slIds)
            {
                SampleLine sl = tr.GetObject(slId, OpenMode.ForWrite) as SampleLine;
                
                sl.LeftSwathWidth = newWidth;
                sl.RightSwathWidth = newWidth;
                
                totalUpdated++;
            }
        }
        
        tr.Commit();
        ed.WriteMessage($"\nUpdated {totalUpdated} sample lines to {newWidth:F2}m width");
    }
}
```

### 5. Finding Sample Lines at Specific Station

```csharp
[CommandMethod("FINDSAMPLELINE")]
public void FindSampleLineAtStation()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = acDoc.Editor;
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    
    if (civilDoc == null) return;
    
    // Prompt for station
    PromptDoubleOptions pdo = new PromptDoubleOptions("\nEnter station to find: ");
    pdo.AllowNegative = false;
    
    PromptDoubleResult pdr = ed.GetDouble(pdo);
    if (pdr.Status != PromptStatus.OK) return;
    
    double searchStation = pdr.Value;
    double tolerance = 0.01; // 1cm tolerance
    
    using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
    {
        ObjectIdCollection slgIds = civilDoc.GetSampleLineGroupIds();
        bool found = false;
        
        foreach (ObjectId slgId in slgIds)
        {
            SampleLineGroup slg = tr.GetObject(slgId, OpenMode.ForRead) as SampleLineGroup;
            ObjectIdCollection slIds = slg.GetSampleLineIds();
            
            foreach (ObjectId slId in slIds)
            {
                SampleLine sl = tr.GetObject(slId, OpenMode.ForRead) as SampleLine;
                
                if (Math.Abs(sl.Station - searchStation) < tolerance)
                {
                    ed.WriteMessage($"\n✓ Found: {sl.Name}");
                    ed.WriteMessage($"\n  Group: {slg.Name}");
                    ed.WriteMessage($"\n  Station: {sl.Station:F3}");
                    ed.WriteMessage($"\n  Left Width: {sl.LeftSwathWidth:F2}");
                    ed.WriteMessage($"\n  Right Width: {sl.RightSwathWidth:F2}");
                    
                    found = true;
                }
            }
        }
        
        if (!found)
        {
            ed.WriteMessage($"\n✗ No sample line found at station {searchStation:F2}");
        }
        
        tr.Commit();
    }
}
```

### 6. Sample Line Statistics Report

```csharp
[CommandMethod("SLSTATS")]
public void SampleLineStatistics()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = acDoc.Editor;
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    
    if (civilDoc == null) return;
    
    using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
    {
        ObjectIdCollection slgIds = civilDoc.GetSampleLineGroupIds();
        
        ed.WriteMessage("\n╔════════════════════════════════════════╗");
        ed.WriteMessage("\n║   SAMPLE LINE STATISTICS REPORT        ║");
        ed.WriteMessage("\n╚════════════════════════════════════════╝");
        
        int totalSampleLines = 0;
        double minStation = double.MaxValue;
        double maxStation = double.MinValue;
        double avgLeftWidth = 0;
        double avgRightWidth = 0;
        
        foreach (ObjectId slgId in slgIds)
        {
            SampleLineGroup slg = tr.GetObject(slgId, OpenMode.ForRead) as SampleLineGroup;
            ObjectIdCollection slIds = slg.GetSampleLineIds();
            
            foreach (ObjectId slId in slIds)
            {
                SampleLine sl = tr.GetObject(slId, OpenMode.ForRead) as SampleLine;
                
                totalSampleLines++;
                minStation = Math.Min(minStation, sl.Station);
                maxStation = Math.Max(maxStation, sl.Station);
                avgLeftWidth += sl.LeftSwathWidth;
                avgRightWidth += sl.RightSwathWidth;
            }
        }
        
        if (totalSampleLines > 0)
        {
            avgLeftWidth /= totalSampleLines;
            avgRightWidth /= totalSampleLines;
            
            ed.WriteMessage($"\n\nTotal Sample Lines: {totalSampleLines}");
            ed.WriteMessage($"\nStation Range: {minStation:F2} to {maxStation:F2}");
            ed.WriteMessage($"\nAverage Left Width: {avgLeftWidth:F2}m");
            ed.WriteMessage($"\nAverage Right Width: {avgRightWidth:F2}m");
            ed.WriteMessage($"\nTotal Coverage: {maxStation - minStation:F2}m");
        }
        else
        {
            ed.WriteMessage("\n\nNo sample lines found in drawing");
        }
        
        tr.Commit();
    }
}
```

## Sample Line Workflow

```mermaid
graph LR
    A[Alignment] --> B[Create SampleLineGroup]
    B --> C[Add SampleLines]
    C --> D[Set Swath Widths]
    D --> E[Sample Surfaces/Corridors]
    E --> F[Create Section Views]
```

## Best Practices

1. **Consistent naming**: Use station-based names (e.g., "SL_0+025")
2. **Appropriate widths**: Set swath widths to capture all relevant features
3. **Regular intervals**: Place at consistent stations for uniform cross-sections
4. **Group organization**: Keep related sample lines in the same group
5. **Update after changes**: Regenerate if alignment or surfaces change

## Common Patterns

### Pattern 1: Create at Regular Intervals
```csharp
for (double sta = startSta; sta <= endSta; sta += interval)
{
    ObjectId slId = slg.AddSampleLineAt($"SL_{sta:F0}", sta);
}
```

### Pattern 2: Set Uniform Widths
```csharp
SampleLine sl = tr.GetObject(slId, OpenMode.ForWrite) as SampleLine;
sl.LeftSwathWidth = 50.0;
sl.RightSwathWidth = 50.0;
```

### Pattern 3: Iterate All Sample Lines
```csharp
foreach (ObjectId slgId in civilDoc.GetSampleLineGroupIds())
{
    SampleLineGroup slg = tr.GetObject(slgId, OpenMode.ForRead) as SampleLineGroup;
    foreach (ObjectId slId in slg.GetSampleLineIds())
    {
        SampleLine sl = tr.GetObject(slId, OpenMode.ForRead) as SampleLine;
        // Process
    }
}
```

## Related Classes

- [SampleLineGroup](SampleLineGroup.md) - Container for sample lines
- [Alignment](../Alignment/Alignment.md) - Parent alignment
- [Surface](../Surface/Surface.md) - Surfaces sampled by sample lines
- [Corridor](../Corridor/Corridor.md) - Corridors sampled by sample lines
- [CivilDocument](../Core/CivilDocument.md) - Document container

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-SampleLine)
- [Cross Sections Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-CrossSections)
