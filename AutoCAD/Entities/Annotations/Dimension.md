# Dimension Class

## Overview
The `Dimension` class is the base class for all dimension entities in AutoCAD including linear, aligned, rotated, angular, radial, and diameter dimensions.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Dimension
                  ├─ AlignedDimension
                  ├─ RotatedDimension
                  ├─ RadialDimension
                  ├─ DiametricDimension
                  └─ AngularDimension
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `DimensionText` | `string` | Gets/sets the dimension text override |
| `Measurement` | `double` | Gets the measured value |
| `DimensionStyle` | `ObjectId` | Gets/sets the dimension style |
| `TextPosition` | `Point3d` | Gets/sets the text position |
| `TextRotation` | `double` | Gets/sets the text rotation |

## Code Examples

### Example 1: Creating an Aligned Dimension
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d pt1 = new Point3d(0, 0, 0);
    Point3d pt2 = new Point3d(100, 50, 0);
    Point3d dimLinePoint = new Point3d(50, 75, 0);
    
    AlignedDimension dim = new AlignedDimension(pt1, pt2, dimLinePoint, "", db.Dimstyle);
    
    btr.AppendEntity(dim);
    tr.AddNewlyCreatedDBObject(dim, true);
    
    tr.Commit();
}
```

## Related Objects
- [Entity](../../BaseClasses/Entity.md) - Base class
- [DimStyleTable](../../SymbolTables/DimStyleTable.md) - Dimension styles

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Dimension)
