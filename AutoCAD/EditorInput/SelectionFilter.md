# SelectionFilter Class

## Overview
The `SelectionFilter` class filters entity selection based on DXF codes and values. Essential for selecting specific entity types, layers, colors, and other properties.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Constructor
```csharp
SelectionFilter(TypedValue[] filterList)
```

## Code Example
```csharp
using Autodesk.AutoCAD.EditorInput;

// Filter for lines only
TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "LINE")
};
SelectionFilter filter = new SelectionFilter(filterList);

PromptSelectionResult result = ed.GetSelection(filter);
```

## Common Filters
- **Entity Type:** `DxfCode.Start` + entity name ("LINE", "CIRCLE", etc.)
- **Layer:** `DxfCode.LayerName` + layer name
- **Color:** `DxfCode.Color` + color index
- **Linetype:** `DxfCode.LinetypeName` + linetype name

## Related Classes
- **TypedValue** - DXF code/value pairs
- **PromptSelectionOptions** - Selection options
- **Editor** - GetSelection methods

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
