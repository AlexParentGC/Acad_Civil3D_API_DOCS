# PromptSelectionOptions Class

## Overview
The `PromptSelectionOptions` class defines options for prompting the user to select entities in AutoCAD. It provides extensive control over selection behavior, including keywords, filters, and selection modes.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Inheritance Hierarchy
```
System.Object
  └─ PromptOptions
      └─ PromptSelectionOptions
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Message` | `string` | Gets or sets the prompt message displayed to the user |
| `MessageForAdding` | `string` | Message displayed when adding objects to selection |
| `MessageForRemoval` | `string` | Message displayed when removing objects from selection |
| `Keywords` | `PromptSelectionKeywordCollection` | Collection of keywords available during selection |
| `KeywordInput` | `string` | Gets the keyword entered by the user |
| `AllowDuplicates` | `bool` | Allows selecting the same object multiple times |
| `SingleOnly` | `bool` | Limits selection to a single object |
| `SinglePickInSpace` | `bool` | Allows single pick in model space only |
| `SelectEverythingInAperture` | `bool` | Selects all objects within the pickbox |
| `RejectObjectsFromNonCurrentSpace` | `bool` | Rejects objects not in current space |
| `RejectObjectsOnLockedLayers` | `bool` | Rejects objects on locked layers |
| `RejectPaperspaceViewport` | `bool` | Rejects paperspace viewports |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SetRejectMessage(string)` | `void` | Sets message displayed when selection is rejected |
| `AddAllowedClass(Type, bool)` | `void` | Adds allowed object type for selection |

## Code Examples

### Example 1: Basic Selection Prompt
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;

Document doc = Application.DocumentManager.MdiActiveDocument;
Editor ed = doc.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect objects: ";

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    SelectionSet selSet = result.Value;
    ed.WriteMessage($"\n{selSet.Count} objects selected");
}
```

### Example 2: Selection with Keywords
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect objects or [All/Window/Crossing]: ";

// Add keywords
opts.Keywords.Add("All");
opts.Keywords.Add("Window");
opts.Keywords.Add("Crossing");
opts.Keywords.Default = "All";

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} objects selected");
}
else if (result.Status == PromptStatus.Keyword)
{
    string keyword = result.StringResult;
    ed.WriteMessage($"\nKeyword entered: {keyword}");
    
    if (keyword == "All")
    {
        // Select all objects
        result = ed.SelectAll();
    }
}
```

### Example 3: Single Object Selection
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect a single object: ";
opts.SingleOnly = true;  // Limit to single selection
opts.SinglePickInSpace = true;  // Single pick in model space

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ObjectId[] ids = result.Value.GetObjectIds();
    ed.WriteMessage($"\nSelected object ID: {ids[0]}");
}
```

### Example 4: Selection with Rejection Rules
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect objects (unlocked layers only): ";
opts.RejectObjectsOnLockedLayers = true;
opts.SetRejectMessage("\nObject is on a locked layer!");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} objects selected from unlocked layers");
}
```

### Example 5: Selection with Type Filtering
```csharp
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect lines only: ";

// Allow only Line objects
opts.AddAllowedClass(typeof(Line), true);
opts.SetRejectMessage("\nOnly lines can be selected!");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} lines selected");
}
```

### Example 6: Advanced Selection with Multiple Options
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect objects: ";
opts.MessageForAdding = "\nAdd objects: ";
opts.MessageForRemoval = "\nRemove objects: ";

// Configure selection behavior
opts.AllowDuplicates = false;
opts.RejectObjectsFromNonCurrentSpace = true;
opts.RejectPaperspaceViewport = true;
opts.SelectEverythingInAperture = false;

// Add keywords for selection modes
opts.Keywords.Add("Window");
opts.Keywords.Add("Crossing");
opts.Keywords.Add("Fence");
opts.Keywords.Add("All");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    SelectionSet selSet = result.Value;
    ed.WriteMessage($"\n{selSet.Count} unique objects selected");
    
    // Process selection
    foreach (SelectedObject selObj in selSet)
    {
        ed.WriteMessage($"\nObject ID: {selObj.ObjectId}");
    }
}
else if (result.Status == PromptStatus.Keyword)
{
    string keyword = result.StringResult;
    ed.WriteMessage($"\nSelection mode: {keyword}");
}
```

## Common Patterns

### Selection with Undo
```csharp
PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect objects: ";
opts.Keywords.Add("Undo");

// Handle undo in selection loop
```

### Combining with SelectionFilter
```csharp
PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect circles: ";

TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "CIRCLE")
};
SelectionFilter filter = new SelectionFilter(filterList);

PromptSelectionResult result = ed.GetSelection(opts, filter);
```

## Best Practices

1. **Clear Messages**: Provide clear, descriptive prompt messages
2. **Keywords**: Use keywords for common selection modes (All, Window, Crossing)
3. **Rejection Messages**: Set custom rejection messages for better user feedback
4. **Single Selection**: Use `SingleOnly` when only one object is needed
5. **Layer Filtering**: Use `RejectObjectsOnLockedLayers` to prevent locked layer selection
6. **Type Filtering**: Combine with `SelectionFilter` for complex filtering
7. **User Feedback**: Always check `PromptStatus` before processing results

## Related Classes
- **PromptSelectionResult** - Result of selection prompt
- **SelectionFilter** - Filter selection by entity properties
- **SelectionSet** - Collection of selected objects
- **SelectedObject** - Individual selected object
- **Editor** - Editor class with GetSelection methods

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [PromptSelectionOptions Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_EditorInput_PromptSelectionOptions)
