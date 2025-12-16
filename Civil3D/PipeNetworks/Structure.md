# Structure Class

## Overview
The `Structure` class represents a structure (manhole, inlet, outlet, etc.) in a Civil3D pipe network.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Structure
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets the structure name |
| `NetworkId` | `ObjectId` | Gets the parent network ObjectId |
| `Location` | `Point3d` | Gets the 3D location |
| `SumpElevation` | `double` | Gets/sets the sump elevation |
| `RimElevation` | `double` | Gets/sets the rim elevation |
| `ConnectedPipesCount` | `int` | Gets the number of connected pipes |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetConnectedPipeIds()` | `ObjectIdCollection` | Gets all connected pipe ObjectIds |

## Code Examples

### Example 1: Listing Structure Properties
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
    ObjectIdCollection structureIds = network.GetStructureIds();
    
    foreach (ObjectId structId in structureIds)
    {
        Structure structure = tr.GetObject(structId, OpenMode.ForRead) as Structure;
        
        ed.WriteMessage($"\nStructure: {structure.Name}");
        ed.WriteMessage($"\n  Location: ({structure.Location.X:F2}, {structure.Location.Y:F2})");
        ed.WriteMessage($"\n  Rim Elevation: {structure.RimElevation:F2}");
        ed.WriteMessage($"\n  Sump Elevation: {structure.SumpElevation:F2}");
        ed.WriteMessage($"\n  Connected Pipes: {structure.ConnectedPipesCount}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Network](Network.md) - Parent pipe network
- [Pipe](Pipe.md) - Connected pipes
- [CivilDocument](../Core/CivilDocument.md) - Document container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Structure)
