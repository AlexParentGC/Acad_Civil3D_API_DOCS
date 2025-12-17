# PromptStringOptions Class

## Overview
Defines options for prompting the user to enter a string value.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `DefaultValue` - Default string value
- `AllowSpaces` - Allow spaces in input
- `UseDefaultValue` - Use default value

## Code Example
```csharp
PromptStringOptions pso = new PromptStringOptions("\nEnter name: ");
pso.AllowSpaces = true;
pso.DefaultValue = "Default";
PromptResult pr = ed.GetString(pso);
if (pr.Status == PromptStatus.OK)
{
    string name = pr.StringResult;
}
```

## Related Classes
- PromptResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
