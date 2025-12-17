# TypedValue Structure

## Overview
Represents a DXF code and value pair used for entity filtering and extended data.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Constructor
```csharp
TypedValue(int typeCode, object value)
```

## Key Properties
- `TypeCode` - DXF code (int)
- `Value` - Associated value (object)

## Code Example
```csharp
// Filter for lines on layer "0"
TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "LINE"),
    new TypedValue((int)DxfCode.LayerName, "0")
};
SelectionFilter filter = new SelectionFilter(filterList);
```

## Common DXF Codes
- `0` (Start) - Entity type
- `8` (LayerName) - Layer name
- `62` (Color) - Color index
- `6` (LinetypeName) - Linetype name

## Related Classes
- SelectionFilter, DxfCode

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
