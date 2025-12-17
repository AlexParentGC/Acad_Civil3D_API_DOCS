# PromptDoubleOptions Class

## Overview
Defines options for prompting the user to enter a double value.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `DefaultValue` - Default value
- `AllowNegative` - Allow negative values
- `AllowZero` - Allow zero
- `AllowNone` - Allow null response
- `UseDefaultValue` - Use default value

## Code Example
```csharp
PromptDoubleOptions pdo = new PromptDoubleOptions("\nEnter value: ");
pdo.AllowNegative = false;
pdo.DefaultValue = 10.0;
PromptDoubleResult pdr = ed.GetDouble(pdo);
if (pdr.Status == PromptStatus.OK)
{
    double value = pdr.Value;
}
```

## Related Classes
- PromptDoubleResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
