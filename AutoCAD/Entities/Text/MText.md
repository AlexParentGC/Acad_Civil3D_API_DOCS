# MText Class

## Overview
The `MText` class represents multi-line text in AutoCAD with rich formatting capabilities including fonts, colors, stacking, and more.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ MText
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Contents` | `string` | Gets/sets the text content (with formatting codes) |
| `Text` | `string` | Gets the plain text without formatting |
| `Location` | `Point3d` | Gets/sets the insertion point |
| `TextHeight` | `double` | Gets/sets the text height |
| `Width` | `double` | Gets/sets the text boundary width |
| `ActualWidth` | `double` | Gets the actual width of the text |
| `ActualHeight` | `double` | Gets the actual height of the text |
| `Rotation` | `double` | Gets/sets the rotation angle (radians) |
| `Attachment` | `AttachmentPoint` | Gets/sets the attachment point |
| `FlowDirection` | `FlowDirection` | Gets/sets text flow direction |
| `TextStyleId` | `ObjectId` | Gets/sets the text style |
| `LineSpacingFactor` | `double` | Gets/sets line spacing factor |
| `LineSpacingStyle` | `LineSpacingStyle` | Gets/sets line spacing style |
| `BackgroundFill` | `bool` | Gets/sets background fill enabled |

## Code Examples

### Example 1: Creating Simple MText
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MText mtext = new MText();
    mtext.Location = new Point3d(100, 100, 0);
    mtext.TextHeight = 5.0;
    mtext.Width = 200.0; // Text boundary width
    mtext.Contents = "This is multi-line text.\\PIt can span multiple lines.";
    
    btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    tr.Commit();
}
```

### Example 2: Creating Formatted MText
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MText mtext = new MText();
    mtext.Location = new Point3d(100, 100, 0);
    mtext.TextHeight = 5.0;
    mtext.Width = 300.0;
    
    // Use formatting codes:
    // \\P = new paragraph
    // \\C# = color (1=red, 2=yellow, etc.)
    // \\f = font
    // \\H = height multiplier
    mtext.Contents = "\\C1;Red Text\\C;\\PNormal Text\\P\\H2x;Large Text";
    
    btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    tr.Commit();
}
```

### Example 3: Setting Attachment Point
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MText mtext = new MText();
    mtext.Location = new Point3d(200, 200, 0);
    mtext.TextHeight = 5.0;
    mtext.Width = 150.0;
    mtext.Contents = "Middle Center Text";
    
    // Set attachment to middle center
    mtext.Attachment = AttachmentPoint.MiddleCenter;
    
    btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    tr.Commit();
}
```

## Common Formatting Codes

| Code | Description |
|------|-------------|
| `\\P` | New paragraph (line break) |
| `\\p` | New paragraph without line break |
| `\\C#;` | Color (1-255) |
| `\\C;` | Reset to default color |
| `\\f` | Font change |
| `\\H#x;` | Height multiplier |
| `\\W#;` | Width factor |
| `\\Q#;` | Oblique angle |
| `\\T#;` | Tracking |
| `\\L` | Underline on |
| `\\l` | Underline off |
| `\\O` | Overline on |
| `\\o` | Overline off |

## Related Objects
- [DBText](DBText.md) - Single-line text
- [TextStyleTable](../../SymbolTables/TextStyleTable.md) - Text style definitions
- [Entity](../../BaseClasses/Entity.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_MText)
