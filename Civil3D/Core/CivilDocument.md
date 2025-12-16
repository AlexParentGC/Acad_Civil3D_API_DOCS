# CivilDocument Class

## Overview
The `CivilDocument` class is the central object for accessing all Civil3D-specific objects in a drawing. It provides collections and methods for working with alignments, surfaces, pipe networks, and other Civil3D entities.

## Namespace
`Autodesk.Civil.ApplicationServices`

## Inheritance Hierarchy
```
System.Object
  └─ CivilDocument
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Database` | `Database` | Gets the associated AutoCAD database |
| `Name` | `string` | Gets the document name |
| `Settings` | `SettingsCivil3D` | Gets the Civil3D settings |
| `Styles` | `StylesRoot` | Gets the Civil3D styles root |

## Key Methods - Object Collections

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetAlignmentIds()` | `ObjectIdCollection` | Gets all alignment ObjectIds |
| `GetSurfaceIds()` | `ObjectIdCollection` | Gets all surface ObjectIds |
| `GetPipeNetworkIds()` | `ObjectIdCollection` | Gets all pipe network ObjectIds |
| `GetCorridorIds()` | `ObjectIdCollection` | Gets all corridor ObjectIds |
| `GetAssemblyIds()` | `ObjectIdCollection` | Gets all assembly ObjectIds |
| `GetCatchmentIds()` | `ObjectIdCollection` | Gets all catchment ObjectIds |
| `GetGradingIds()` | `ObjectIdCollection` | Gets all grading ObjectIds |
| `GetFeatureLineIds()` | `ObjectIdCollection` | Gets all feature line ObjectIds |
| `GetSampleLineGroupIds()` | `ObjectIdCollection` | Gets all sample line group ObjectIds |

## Code Examples

### Example 1: Getting CivilDocument
```csharp
using Autodesk.Civil.ApplicationServices;
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
CivilDocument civilDoc = CivilDocument.GetCivilDocument(acDoc.Database);

// Or use the active document directly
CivilDocument civilDoc2 = CivilApplication.ActiveDocument;
```

### Example 2: Listing All Alignments
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    
    ed.WriteMessage($"\nFound {alignmentIds.Count} alignments:");
    
    foreach (ObjectId alignId in alignmentIds)
    {
        Alignment alignment = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
        ed.WriteMessage($"\n  {alignment.Name} - Length: {alignment.Length:F2}");
    }
    
    tr.Commit();
}
```

### Example 3: Listing All Surfaces
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    
    ed.WriteMessage($"\nFound {surfaceIds.Count} surfaces:");
    
    foreach (ObjectId surfId in surfaceIds)
    {
        Surface surface = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
        
        string surfaceType = surface.GetType().Name;
        ed.WriteMessage($"\n  {surface.Name} ({surfaceType})");
        
        if (surface is TinSurface tinSurf)
        {
            ed.WriteMessage($" - Points: {tinSurf.GetGeneralProperties().NumberOfPoints}");
        }
    }
    
    tr.Commit();
}
```

### Example 4: Working with COGO Points
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    CogoPointCollection cogoPoints = civilDoc.CogoPoints;
    
    // Get all point numbers
    ObjectIdCollection pointIds = cogoPoints.GetPointIds();
    
    ed.WriteMessage($"\nFound {pointIds.Count} COGO points:");
    
    foreach (ObjectId pointId in pointIds)
    {
        CogoPoint point = tr.GetObject(pointId, OpenMode.ForRead) as CogoPoint;
        ed.WriteMessage($"\n  Point {point.PointNumber}: ({point.Easting:F2}, {point.Northing:F2}, {point.Elevation:F2})");
    }
    
    tr.Commit();
}
```

### Example 5: Accessing Pipe Networks
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection networkIds = civilDoc.GetPipeNetworkIds();
    
    foreach (ObjectId networkId in networkIds)
    {
        Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
        
        ed.WriteMessage($"\nNetwork: {network.Name}");
        
        // Get pipes in network
        ObjectIdCollection pipeIds = network.GetPipeIds();
        ed.WriteMessage($"\n  Pipes: {pipeIds.Count}");
        
        // Get structures in network
        ObjectIdCollection structureIds = network.GetStructureIds();
        ed.WriteMessage($"\n  Structures: {structureIds.Count}");
    }
    
    tr.Commit();
}
```

## Common Patterns

### Checking if Civil3D Objects Exist
```csharp
Document acDoc = Application.DocumentManager.MdiActiveDocument;
CivilDocument civilDoc = null;

try
{
    civilDoc = CivilDocument.GetCivilDocument(acDoc.Database);
    
    if (civilDoc != null)
    {
        // Civil3D is available
        ObjectIdCollection alignments = civilDoc.GetAlignmentIds();
        bool hasAlignments = alignments.Count > 0;
    }
}
catch (System.Exception)
{
    // Civil3D not available or no Civil3D objects
}
```

### Iterating Through All Civil3D Object Types
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ed.WriteMessage("\n=== Civil3D Objects Summary ===");
    ed.WriteMessage($"\nAlignments: {civilDoc.GetAlignmentIds().Count}");
    ed.WriteMessage($"\nSurfaces: {civilDoc.GetSurfaceIds().Count}");
    ed.WriteMessage($"\nPipe Networks: {civilDoc.GetPipeNetworkIds().Count}");
    ed.WriteMessage($"\nCorridors: {civilDoc.GetCorridorIds().Count}");
    ed.WriteMessage($"\nAssemblies: {civilDoc.GetAssemblyIds().Count}");
    ed.WriteMessage($"\nCatchments: {civilDoc.GetCatchmentIds().Count}");
    ed.WriteMessage($"\nGradings: {civilDoc.GetGradingIds().Count}");
    ed.WriteMessage($"\nFeature Lines: {civilDoc.GetFeatureLineIds().Count}");
    ed.WriteMessage($"\nCOGO Points: {civilDoc.CogoPoints.Count}");
    
    tr.Commit();
}
```

## Related Objects
- [CivilApplication](CivilApplication.md) - Civil3D application object
- [Alignment](../Alignment/Alignment.md) - Horizontal alignment
- [Surface](../Surface/Surface.md) - Terrain surface
- [Network](../PipeNetworks/Network.md) - Pipe network
- [CogoPoint](../Points/CogoPoint.md) - Survey point

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-CivilDocument)
