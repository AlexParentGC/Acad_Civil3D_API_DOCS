# Color Class

## Overview
The `Color` class represents a color in AutoCAD. It provides methods for creating and manipulating colors using various color models including ACI (AutoCAD Color Index), RGB, and color books.

## Namespace
`Autodesk.AutoCAD.Colors`

## Inheritance Hierarchy
```
System.Object
  └─ Color
```

## Static Factory Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `FromColorIndex(ColorMethod, short)` | `Color` | Creates color from ACI index |
| `FromRgb(byte, byte, byte)` | `Color` | Creates color from RGB values |
| `FromNames(string, string)` | `Color` | Creates color from color book |
| `FromEntityColor(EntityColor)` | `Color` | Creates color from EntityColor |

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ColorMethod` | `ColorMethod` | Gets the color method (ByLayer, ByBlock, ByACI, ByColor) |
| `ColorIndex` | `short` | Gets or sets the ACI color index (1-255) |
| `Red` | `byte` | Gets the red component (0-255) |
| `Green` | `byte` | Gets the green component (0-255) |
| `Blue` | `byte` | Gets the blue component (0-255) |
| `ColorName` | `string` | Gets the color name from color book |
| `BookName` | `string` | Gets the color book name |
| `IsByLayer` | `bool` | Checks if color is ByLayer |
| `IsByBlock` | `bool` | Checks if color is ByBlock |
| `IsByAci` | `bool` | Checks if color is by ACI index |
| `IsByColor` | `bool` | Checks if color is true color (RGB) |

## Code Examples

### Example 1: Creating Colors by ACI Index
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

// Create color by ACI index
Color red = Color.FromColorIndex(ColorMethod.ByAci, 1);    // Red
Color yellow = Color.FromColorIndex(ColorMethod.ByAci, 2); // Yellow
Color green = Color.FromColorIndex(ColorMethod.ByAci, 3);  // Green
Color cyan = Color.FromColorIndex(ColorMethod.ByAci, 4);   // Cyan
Color blue = Color.FromColorIndex(ColorMethod.ByAci, 5);   // Blue

// Apply to entity
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = new Line(new Point3d(0, 0, 0), new Point3d(100, 0, 0));
    line.Color = red;
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    tr.Commit();
}
```

### Example 2: Creating RGB Colors
```csharp
using Autodesk.AutoCAD.Colors;

// Create true color from RGB
Color orange = Color.FromRgb(255, 165, 0);
Color purple = Color.FromRgb(128, 0, 128);
Color pink = Color.FromRgb(255, 192, 203);

ed.WriteMessage($"\nOrange RGB: R={orange.Red}, G={orange.Green}, B={orange.Blue}");
```

### Example 3: ByLayer and ByBlock Colors
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

// ByLayer color (inherits from layer)
Color byLayer = Color.FromColorIndex(ColorMethod.ByLayer, 0);

// ByBlock color (inherits from block)
Color byBlock = Color.FromColorIndex(ColorMethod.ByBlock, 0);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = new Circle(new Point3d(0, 0, 0), Vector3d.ZAxis, 50);
    circle.Color = byLayer; // Will use layer color
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}
```

### Example 4: Checking Color Properties
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(objectId, OpenMode.ForRead) as Entity;
    Color color = ent.Color;
    
    if (color.IsByLayer)
    {
        ed.WriteMessage("\nColor is ByLayer");
    }
    else if (color.IsByBlock)
    {
        ed.WriteMessage("\nColor is ByBlock");
    }
    else if (color.IsByAci)
    {
        ed.WriteMessage($"\nColor is ACI index: {color.ColorIndex}");
    }
    else if (color.IsByColor)
    {
        ed.WriteMessage($"\nColor is RGB: ({color.Red}, {color.Green}, {color.Blue})");
    }
    
    tr.Commit();
}
```

### Example 5: Changing Entity Colors
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
Database db = ed.Document.Database;

PromptSelectionResult result = ed.GetSelection();

if (result.Status == PromptStatus.OK)
{
    Color newColor = Color.FromRgb(255, 0, 0); // Red
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        foreach (SelectedObject selObj in result.Value)
        {
            Entity ent = tr.GetObject(selObj.ObjectId, OpenMode.ForWrite) as Entity;
            ent.Color = newColor;
        }
        tr.Commit();
    }
    
    ed.WriteMessage($"\n{result.Value.Count} objects changed to red");
}
```

### Example 6: Color Book Colors
```csharp
using Autodesk.AutoCAD.Colors;

// Create color from color book
Color pantoneColor = Color.FromNames("PANTONE", "PANTONE 185 C");

if (pantoneColor != null)
{
    ed.WriteMessage($"\nColor book: {pantoneColor.BookName}");
    ed.WriteMessage($"\nColor name: {pantoneColor.ColorName}");
}
```

## Common ACI Color Indices

| Index | Color Name |
|-------|------------|
| 0 | ByBlock |
| 1 | Red |
| 2 | Yellow |
| 3 | Green |
| 4 | Cyan |
| 5 | Blue |
| 6 | Magenta |
| 7 | White/Black (depends on background) |
| 8-255 | Various colors |
| 256 | ByLayer |

## Best Practices

1. **ByLayer**: Use ByLayer for most entities to maintain layer-based organization
2. **True Color**: Use RGB for precise color matching
3. **ACI Colors**: Use ACI for compatibility with older AutoCAD versions
4. **Color Books**: Use for industry-standard colors (Pantone, RAL, etc.)
5. **Check Method**: Always check `ColorMethod` before accessing color properties
6. **Null Checks**: Color book colors may return null if not found

## Related Classes
- **EntityColor** - Entity color properties
- **ColorMethod** - Color method enumeration
- **Transparency** - Transparency settings
- **Entity** - Base class with Color property
- **Layer** - Layer with color property

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Color Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Colors_Color)
