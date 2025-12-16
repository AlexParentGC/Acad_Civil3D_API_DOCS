# Alignment Class

## Overview
The `Alignment` class represents a horizontal alignment in Civil3D, typically used for road centerlines, pipelines, or any linear feature. It can contain curves, lines, and spirals.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Alignment
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the alignment name |
| `Description` | `string` | Gets/sets the description |
| `Length` | `double` | Gets the total length |
| `StartingStation` | `double` | Gets/sets the starting station value |
| `EndingStation` | `double` | Gets the ending station value |
| `SiteId` | `ObjectId` | Gets/sets the parent site ObjectId |
| `StyleId` | `ObjectId` | Gets/sets the alignment style |
| `Entities` | `AlignmentEntityCollection` | Gets the collection of alignment entities |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetProfileViewIds()` | `ObjectIdCollection` | Gets all profile view ObjectIds |
| `GetProfileIds()` | `ObjectIdCollection` | Gets all profile ObjectIds |
| `StationOffset(double, double, out double, out double)` | `void` | Converts XY to station/offset |
| `PointLocation(double, double)` | `Point3d` | Gets XY from station/offset |
| `GetStationAtPoint(Point3d)` | `double` | Gets station at a point |

## Code Examples

### Example 1: Accessing an Alignment
```csharp
using Autodesk.Civil.DatabaseServices;

using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    
    if (alignmentIds.Count > 0)
    {
        Alignment alignment = tr.GetObject(alignmentIds[0], OpenMode.ForRead) as Alignment;
        
        ed.WriteMessage($"\nAlignment: {alignment.Name}");
        ed.WriteMessage($"\nLength: {alignment.Length:F2}");
        ed.WriteMessage($"\nStart Station: {alignment.StartingStation:F2}");
        ed.WriteMessage($"\nEnd Station: {alignment.EndingStation:F2}");
    }
    
    tr.Commit();
}
```

### Example 2: Getting Point from Station/Offset
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    double station = 100.0;
    double offset = 5.0; // 5 units to the right
    
    Point3d point = alignment.PointLocation(station, offset);
    
    ed.WriteMessage($"\nPoint at Station {station}, Offset {offset}:");
    ed.WriteMessage($"\n  X: {point.X:F2}, Y: {point.Y:F2}");
    
    tr.Commit();
}
```

### Example 3: Converting XY to Station/Offset
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    Point3d testPoint = new Point3d(1000, 2000, 0);
    
    double station, offset;
    alignment.StationOffset(testPoint.X, testPoint.Y, out station, out offset);
    
    ed.WriteMessage($"\nPoint ({testPoint.X:F2}, {testPoint.Y:F2}):");
    ed.WriteMessage($"\n  Station: {station:F2}");
    ed.WriteMessage($"\n  Offset: {offset:F2}");
    
    tr.Commit();
}
```

### Example 4: Iterating Through Alignment Entities
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    foreach (AlignmentEntity entity in alignment.Entities)
    {
        ed.WriteMessage($"\nEntity Type: {entity.EntityType}");
        ed.WriteMessage($"\n  Start Station: {entity.StartStation:F2}");
        ed.WriteMessage($"\n  End Station: {entity.EndStation:F2}");
        ed.WriteMessage($"\n  Length: {entity.Length:F2}");
        
        if (entity.EntityType == AlignmentEntityType.Arc)
        {
            AlignmentArc arc = entity as AlignmentArc;
            ed.WriteMessage($"\n  Radius: {arc.Radius:F2}");
        }
    }
    
    tr.Commit();
}
```

### Example 5: Getting Associated Profiles
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    ObjectIdCollection profileIds = alignment.GetProfileIds();
    
    ed.WriteMessage($"\nProfiles for alignment '{alignment.Name}':");
    
    foreach (ObjectId profileId in profileIds)
    {
        Profile profile = tr.GetObject(profileId, OpenMode.ForRead) as Profile;
        ed.WriteMessage($"\n  {profile.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Profile](Profile.md) - Vertical profile along alignment
- [CivilDocument](../Core/CivilDocument.md) - Container for alignments
- [Feature](Feature.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Alignment)
