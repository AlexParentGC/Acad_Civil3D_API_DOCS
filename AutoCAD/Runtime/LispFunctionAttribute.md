# LispFunctionAttribute Class

## Overview
Attribute to expose .NET methods to AutoLISP.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Constructor
```csharp
LispFunctionAttribute(string functionName)
```

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

[LispFunction("MyLispFunc")]
public ResultBuffer MyLispFunction(ResultBuffer args)
{
    // Process LISP arguments
    return new ResultBuffer(new TypedValue(5005, "Result"));
}
```

## Related Classes
- CommandMethodAttribute, ResultBuffer

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
