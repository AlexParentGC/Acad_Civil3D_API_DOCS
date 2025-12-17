# AcRx Class

## Overview
Provides runtime utilities and exception handling for AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Methods
- `ErrorStatus()` - Gets error status

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

try
{
    // AutoCAD operation
}
catch (Autodesk.AutoCAD.Runtime.Exception ex)
{
    ErrorStatus es = ex.ErrorStatus;
    ed.WriteMessage($"\nError: {es}");
}
```

## Related Classes
- ErrorStatus, Exception

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
