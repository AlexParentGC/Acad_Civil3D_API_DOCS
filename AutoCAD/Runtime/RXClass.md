# RXClass Class

## Overview
Represents runtime class information for AutoCAD objects.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Properties
- `Name` - Class name
- `DxfName` - DXF name
- `AppName` - Application name

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

RXClass rxClass = RXObject.GetClass(typeof(Line));
ed.WriteMessage($"\nClass: {rxClass.Name}");
ed.WriteMessage($"\nDXF: {rxClass.DxfName}");
```

## Related Classes
- RXObject, DBObject

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
