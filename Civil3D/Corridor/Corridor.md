# Corridor Class

## Overview
The `Corridor` class represents a road corridor in Civil3D, which models a roadway design using alignments, profiles, and assemblies.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Corridor
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the corridor name |
| `Description` | `string` | Gets/sets the description |
| `AlignmentId` | `ObjectId` | Gets/sets the baseline alignment |
| `ProfileId` | `ObjectId` | Gets/sets the baseline profile |
| `StyleId` | `ObjectId` | Gets/sets the corridor style |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetBaselines()` | `BaselineCollection` | Gets the collection of baselines |
| `Rebuild()` | `void` | Rebuilds the corridor |

## Code Examples

### Example 1: Accessing Corridor Properties
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection corridorIds = civilDoc.GetCorridorIds();
    
    foreach (ObjectId corridorId in corridorIds)
    {
        Corridor corridor = tr.GetObject(corridorId, OpenMode.ForRead) as Corridor;
        
        ed.WriteMessage($"\nCorridor: {corridor.Name}");
        ed.WriteMessage($"\nDescription: {corridor.Description}");
        
        // Get baselines
        BaselineCollection baselines = corridor.GetBaselines();
        ed.WriteMessage($"\nNumber of Baselines: {baselines.Count}");
    }
    
    tr.Commit();
}
```

### Example 2: Iterating Through Baselines
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Corridor corridor = tr.GetObject(corridorId, OpenMode.ForRead) as Corridor;
    
    BaselineCollection baselines = corridor.GetBaselines();
    
    foreach (Baseline baseline in baselines)
    {
        ed.WriteMessage($"\nBaseline: {baseline.Name}");
        ed.WriteMessage($"\n  Start Station: {baseline.StartStation:F2}");
        ed.WriteMessage($"\n  End Station: {baseline.EndStation:F2}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Assembly](Assembly.md) - Corridor cross-section template
- [Alignment](../Alignment/Alignment.md) - Baseline alignment
- [Profile](../Alignment/Profile.md) - Baseline profile
- [CivilDocument](../Core/CivilDocument.md) - Container for corridors

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Corridor)
