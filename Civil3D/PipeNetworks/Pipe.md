# Pipe Class

## Overview
The `Pipe` class represents a pipe segment in a Civil3D pipe network.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Pipe
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets the pipe name |
| `NetworkId` | `ObjectId` | Gets the parent network ObjectId |
| `StartPoint` | `Point3d` | Gets the start point (3D) |
| `EndPoint` | `Point3d` | Gets the end point (3D) |
| `Length2D` | `double` | Gets the 2D (horizontal) length |
| `Length3D` | `double` | Gets the 3D (slope) length |
| `InnerDiameterOrWidth` | `double` | Gets/sets the inner diameter |
| `OuterDiameterOrWidth` | `double` | Gets the outer diameter |
| `Slope` | `double` | Gets the slope (rise/run) |
| `StartOffset` | `double` | Gets/sets the start invert offset |
| `EndOffset` | `double` | Gets/sets the end invert offset |
| `FlowDirection` | `FlowDirectionType` | Gets/sets the flow direction |

## Code Examples

### Example 1: Listing Pipe Properties
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
    ObjectIdCollection pipeIds = network.GetPipeIds();
    
    foreach (ObjectId pipeId in pipeIds)
    {
        Pipe pipe = tr.GetObject(pipeId, OpenMode.ForRead) as Pipe;
        
        ed.WriteMessage($"\nPipe: {pipe.Name}");
        ed.WriteMessage($"\n  Diameter: {pipe.InnerDiameterOrWidth:F2}");
        ed.WriteMessage($"\n  2D Length: {pipe.Length2D:F2}");
        ed.WriteMessage($"\n  3D Length: {pipe.Length3D:F2}");
        ed.WriteMessage($"\n  Slope: {pipe.Slope * 100:F2}%");
        ed.WriteMessage($"\n  Flow Direction: {pipe.FlowDirection}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Network](Network.md) - Parent pipe network
- [Structure](Structure.md) - Connected structures
- [CivilDocument](../Core/CivilDocument.md) - Document container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Pipe)
