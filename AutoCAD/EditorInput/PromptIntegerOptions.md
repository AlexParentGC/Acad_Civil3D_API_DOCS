# PromptIntegerOptions Class

## Overview
Defines options for prompting the user to enter an integer value.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `DefaultValue` - Default integer value
- `AllowNegative` - Allow negative values
- `AllowZero` - Allow zero
- `AllowNone` - Allow null response

## Code Example
```csharp
PromptIntegerOptions pio = new PromptIntegerOptions("\nEnter count: ");
pio.AllowNegative = false;
pio.DefaultValue = 1;
PromptIntegerResult pir = ed.GetInteger(pio);
if (pir.Status == PromptStatus.OK)
{
    int count = pir.Value;
}
```

## Related Classes
- PromptIntegerResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
