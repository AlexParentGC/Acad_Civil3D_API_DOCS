# Exception Class

## Overview
Base exception class for AutoCAD .NET API exceptions.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Properties
- `ErrorStatus` - ErrorStatus enumeration value
- `Message` - Exception message

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

try
{
    // AutoCAD operation
}
catch (Autodesk.AutoCAD.Runtime.Exception ex)
{
    ed.WriteMessage($"\nError: {ex.Message}");
    ed.WriteMessage($"\nStatus: {ex.ErrorStatus}");
}
```

## Related Classes
- ErrorStatus, AcRx

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
