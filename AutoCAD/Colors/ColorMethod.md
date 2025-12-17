# ColorMethod Enumeration

## Overview
The `ColorMethod` enumeration specifies how an entity's color is determined in AutoCAD.

## Namespace
`Autodesk.AutoCAD.Colors`

## Values

| Value | Description |
|-------|-------------|
| `ByLayer` | Color inherited from layer (most common) |
| `ByBlock` | Color inherited from block reference |
| `ByAci` | Color specified by AutoCAD Color Index (1-255) |
| `ByColor` | True color specified by RGB values |
| `ByDgnIndex` | Color by DGN index |
| `ByPen` | Color by pen number |
| `Foreground` | Foreground color |

## Code Example
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

// ByLayer (most common)
Color byLayerColor = Color.FromColorIndex(ColorMethod.ByLayer, 0);

// ByACI
Color redColor = Color.FromColorIndex(ColorMethod.ByAci, 1);

// ByColor (RGB)
Color customColor = Color.FromRgb(128, 64, 255);

// Apply to entity
entity.Color = byLayerColor;
```

## Best Practices
- Use `ByLayer` for most entities to maintain layer-based organization
- Use `ByColor` (RGB) for precise color matching
- Use `ByAci` for compatibility with older AutoCAD versions

## Related Classes
- **Color** - Color class
- **EntityColor** - Entity color properties

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
