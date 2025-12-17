# Transparency Class

## Overview
Represents transparency settings for AutoCAD entities.

## Namespace
`Autodesk.AutoCAD.Colors`

## Key Properties
- `Alpha` - Transparency value (0-255, 0=transparent, 255=opaque)
- `IsByLayer` - Transparency by layer
- `IsByBlock` - Transparency by block

## Code Example
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

// 50% transparent
Transparency trans = new Transparency(127);

Entity ent = /* get entity */;
ent.Transparency = trans;
```

## Related Classes
- Color, Entity

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
