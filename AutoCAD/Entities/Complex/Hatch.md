# Hatch Class

## Overview
The `Hatch` class represents a filled area with a pattern or solid fill in AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Hatch
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `PatternName` | `string` | Gets/sets the hatch pattern name |
| `PatternScale` | `double` | Gets/sets the pattern scale |
| `PatternAngle` | `double` | Gets/sets the pattern angle |
| `NumberOfLoops` | `int` | Gets the number of boundary loops |
| `Associative` | `bool` | Gets/sets whether hatch is associative |
| `HatchStyle` | `HatchStyle` | Gets/sets the hatch style |
| `Area` | `double` | Gets the hatch area |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `AppendLoop(HatchLoopTypes, ObjectIdCollection)` | `void` | Adds a boundary loop |
| `SetHatchPattern(HatchPatternType, string)` | `void` | Sets the hatch pattern |
| `EvaluateHatch(bool)` | `void` | Evaluates/regenerates the hatch |

## Code Examples

### Example 1: Creating a Solid Hatch
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create a circle to hatch
    Circle circle = new Circle(new Point3d(100, 100, 0), Vector3d.ZAxis, 50);
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    // Create hatch
    Hatch hatch = new Hatch();
    btr.AppendEntity(hatch);
    tr.AddNewlyCreatedDBObject(hatch, true);
    
    // Set to solid fill
    hatch.SetHatchPattern(HatchPatternType.PreDefined, "SOLID");
    
    // Add boundary
    ObjectIdCollection boundaryIds = new ObjectIdCollection();
    boundaryIds.Add(circle.ObjectId);
    hatch.AppendLoop(HatchLoopTypes.Default, boundaryIds);
    
    // Evaluate
    hatch.EvaluateHatch(true);
    
    tr.Commit();
}
```

## Related Objects
- [Entity](../../BaseClasses/Entity.md) - Base class
- [Polyline](../Polylines/Polyline.md) - Common boundary type

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Hatch)
