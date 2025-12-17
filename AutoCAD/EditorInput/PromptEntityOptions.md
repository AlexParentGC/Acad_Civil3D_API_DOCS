# PromptEntityOptions Class

## Overview
Defines options for prompting the user to select a single entity.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `AllowNone` - Allow null response
- `AllowObjectOnLockedLayer` - Allow locked layer objects
- `Keywords` - Available keywords

## Code Example
```csharp
PromptEntityOptions peo = new PromptEntityOptions("\nSelect entity: ");
peo.SetRejectMessage("\nInvalid selection");
peo.AddAllowedClass(typeof(Line), true);
PromptEntityResult per = ed.GetEntity(peo);
if (per.Status == PromptStatus.OK)
{
    ObjectId id = per.ObjectId;
}
```

## Related Classes
- PromptEntityResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
