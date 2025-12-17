# PromptKeywordOptions Class

## Overview
Defines options for prompting the user to select from a list of keywords.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `Keywords` - Collection of available keywords
- `AllowNone` - Allow null response
- `AllowArbitraryInput` - Allow arbitrary input

## Code Example
```csharp
PromptKeywordOptions pko = new PromptKeywordOptions("\nChoose [Yes/No]: ");
pko.Keywords.Add("Yes");
pko.Keywords.Add("No");
pko.Keywords.Default = "Yes";
PromptResult pr = ed.GetKeyword(pko);
if (pr.Status == PromptStatus.OK)
{
    string keyword = pr.StringResult;
}
```

## Related Classes
- PromptResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
