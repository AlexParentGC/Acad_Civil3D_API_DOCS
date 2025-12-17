# DisposableWrapper Class

## Overview
Wrapper class for disposable objects to ensure proper disposal.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Methods
- `Dispose()` - Disposes wrapped object

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

using (DisposableWrapper wrapper = new DisposableWrapper(obj, true))
{
    // Use object
} // Automatically disposed
```

## Related Classes
- IDisposable

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
