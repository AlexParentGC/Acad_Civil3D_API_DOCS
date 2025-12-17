# CommandClass Class

## Overview
Provides methods for command registration and management.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Methods
- `AddCommand(string, string, string, CommandFlags, Type, string)` - Registers command

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

// Commands are typically registered via CommandMethodAttribute
[CommandMethod("MyCommand")]
public void MyCommandMethod()
{
    // Command implementation
}
```

## Related Classes
- CommandMethodAttribute, CommandFlags

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
