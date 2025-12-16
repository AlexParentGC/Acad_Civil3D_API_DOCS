# Network Class

## Overview
The `Network` class represents a pipe network in Civil3D, containing pipes and structures for storm or sanitary systems.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Network
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the network name |
| `Description` | `string` | Gets/sets the description |
| `ReferenceAlignmentId` | `ObjectId` | Gets/sets the reference alignment |
| `ReferenceSurfaceId` | `ObjectId` | Gets/sets the reference surface |
| `PartsListId` | `ObjectId` | Gets/sets the parts list |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetPipeIds()` | `ObjectIdCollection` | Gets all pipe ObjectIds in network |
| `GetStructureIds()` | `ObjectIdCollection` | Gets all structure ObjectIds in network |

## Code Examples

### Example 1: Accessing Pipe Network
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection networkIds = civilDoc.GetPipeNetworkIds();
    
    foreach (ObjectId networkId in networkIds)
    {
        Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
        
        ed.WriteMessage($"\nNetwork: {network.Name}");
        
        ObjectIdCollection pipeIds = network.GetPipeIds();
        ObjectIdCollection structureIds = network.GetStructureIds();
        
        ed.WriteMessage($"\n  Pipes: {pipeIds.Count}");
        ed.WriteMessage($"\n  Structures: {structureIds.Count}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Pipe](Pipe.md) - Pipe segments in network
- [Structure](Structure.md) - Manholes and inlets in network
- [PartsList](PartsList.md) - Catalog of pipe/structure parts
- [CivilDocument](../Core/CivilDocument.md) - Container for networks

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Network)
