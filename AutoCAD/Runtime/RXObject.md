# RXObject Class

## Overview
Base class for runtime objects in AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Methods
- `GetClass(Type)` - Gets RXClass for type
- `IsKindOf(RXClass)` - Checks if object is kind of class

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

RXClass lineClass = RXObject.GetClass(typeof(Line));
bool isLine = entity.IsKindOf(lineClass);
```

## Related Classes
- RXClass, DBObject

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
