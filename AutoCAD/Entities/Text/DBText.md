# DBText Class

## Overview
The `DBText` class represents single-line text in AutoCAD. It's a simple text entity with basic formatting options.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ DBText
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `TextString` | `string` | Gets/sets the text content |
| `Position` | `Point3d` | Gets/sets the insertion point |
| `AlignmentPoint` | `Point3d` | Gets/sets the alignment point |
| `Height` | `double` | Gets/sets the text height |
| `Rotation` | `double` | Gets/sets the rotation angle (radians) |
| `WidthFactor` | `double` | Gets/sets the width factor |
| `Oblique` | `double` | Gets/sets the oblique angle |
| `TextStyleId` | `ObjectId` | Gets/sets the text style |
| `HorizontalMode` | `TextHorizontalMode` | Gets/sets horizontal justification |
| `VerticalMode` | `TextVerticalMode` | Gets/sets vertical justification |
| `IsMirroredInX` | `bool` | Gets/sets X-axis mirroring |
| `IsMirroredInY` | `bool` | Gets/sets Y-axis mirroring |
| `Normal` | `Vector3d` | Gets/sets the normal vector |
| `Thickness` | `double` | Gets/sets the thickness |

## Code Examples

### Example 1: Creating Simple Text
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    DBText text = new DBText();
    text.Position = new Point3d(100, 100, 0);
    text.Height = 5.0;
    text.TextString = "Hello AutoCAD!";
    
    btr.AppendEntity(text);
    tr.AddNewlyCreatedDBObject(text, true);
    
    tr.Commit();
}
```

### Example 2: Creating Justified Text
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    DBText text = new DBText();
    text.Height = 5.0;
    text.TextString = "Centered Text";
    
    // Set justification
    text.HorizontalMode = TextHorizontalMode.TextCenter;
    text.VerticalMode = TextVerticalMode.TextVerticalMid;
    
    // Alignment point is used when justification is not left-baseline
    text.AlignmentPoint = new Point3d(200, 100, 0);
    
    btr.AppendEntity(text);
    tr.AddNewlyCreatedDBObject(text, true);
    
    tr.Commit();
}
```

### Example 3: Modifying Text Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBText text = tr.GetObject(textId, OpenMode.ForWrite) as DBText;
    
    // Change text content
    text.TextString = "Modified Text";
    
    // Change height
    text.Height = 10.0;
    
    // Rotate 45 degrees
    text.Rotation = Math.PI / 4;
    
    // Make text wider
    text.WidthFactor = 1.5;
    
    // Apply oblique angle (italic effect)
    text.Oblique = 15 * (Math.PI / 180);
    
    tr.Commit();
}
```

## Related Objects
- [MText](MText.md) - Multi-line text with rich formatting
- [TextStyleTable](../../SymbolTables/TextStyleTable.md) - Text style definitions
- [Entity](../../BaseClasses/Entity.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBText)
