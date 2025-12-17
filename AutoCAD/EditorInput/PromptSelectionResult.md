# PromptSelectionResult Class

## Overview
The `PromptSelectionResult` class represents the result of a selection prompt operation. It contains the selection set and status information about the user's response.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Inheritance Hierarchy
```
System.Object
  └─ PromptResult
      └─ PromptSelectionResult
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Status` | `PromptStatus` | Gets the status of the prompt (OK, Cancel, Error, etc.) |
| `Value` | `SelectionSet` | Gets the selection set if Status is OK |
| `StringResult` | `string` | Gets the keyword string if a keyword was entered |

## Code Examples

### Example 1: Basic Selection Result Handling
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.DatabaseServices;

Document doc = Application.DocumentManager.MdiActiveDocument;
Editor ed = doc.Editor;

PromptSelectionResult result = ed.GetSelection();

if (result.Status == PromptStatus.OK)
{
    SelectionSet selSet = result.Value;
    ed.WriteMessage($"\n{selSet.Count} objects selected");
    
    // Get object IDs
    ObjectId[] ids = selSet.GetObjectIds();
    
    // Process selected objects
    using (Transaction tr = doc.TransactionManager.StartTransaction())
    {
        foreach (ObjectId id in ids)
        {
            Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
            ed.WriteMessage($"\nObject type: {ent.GetType().Name}");
        }
        tr.Commit();
    }
}
else if (result.Status == PromptStatus.Cancel)
{
    ed.WriteMessage("\nSelection cancelled by user");
}
else if (result.Status == PromptStatus.Error)
{
    ed.WriteMessage("\nError during selection");
}
```

### Example 2: Handling Keywords in Selection
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSelect objects or [All]: ";
opts.Keywords.Add("All");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} objects selected");
}
else if (result.Status == PromptStatus.Keyword)
{
    string keyword = result.StringResult;
    if (keyword == "All")
    {
        // Select all objects
        result = ed.SelectAll();
        if (result.Status == PromptStatus.OK)
        {
            ed.WriteMessage($"\nAll objects selected: {result.Value.Count}");
        }
    }
}
```

### Example 3: Processing Selection Set
```csharp
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.DatabaseServices;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
Database db = ed.Document.Database;

PromptSelectionResult result = ed.GetSelection();

if (result.Status == PromptStatus.OK)
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        foreach (SelectedObject selObj in result.Value)
        {
            if (selObj != null)
            {
                Entity ent = tr.GetObject(selObj.ObjectId, OpenMode.ForWrite) as Entity;
                
                // Modify entity
                ent.ColorIndex = 1; // Red
                
                ed.WriteMessage($"\nModified: {ent.GetType().Name}");
            }
        }
        tr.Commit();
    }
}
```

## Best Practices

1. **Always Check Status**: Check `Status` property before accessing `Value`
2. **Null Checks**: Selection set can be null if status is not OK
3. **Transaction**: Use transactions when modifying selected objects
4. **Keyword Handling**: Check for `PromptStatus.Keyword` when using keywords
5. **Error Handling**: Handle Cancel and Error statuses appropriately

## Related Classes
- **PromptSelectionOptions** - Options for selection prompt
- **SelectionSet** - Collection of selected objects
- **SelectedObject** - Individual selected object
- **PromptStatus** - Status enumeration
- **Editor** - Editor class with GetSelection methods

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [PromptSelectionResult Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_EditorInput_PromptSelectionResult)
