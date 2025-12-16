# Editor Class

## Overview
The `Editor` class provides methods for user interaction, command-line input/output, and entity selection in AutoCAD.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Inheritance Hierarchy
```
System.Object
  └─ Editor
```

## Key Methods - Output

| Method | Return Type | Description |
|--------|-------------|-------------|
| `WriteMessage(string)` | `void` | Writes message to command line |
| `WriteMessage(string, params object[])` | `void` | Writes formatted message |

## Key Methods - Input

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetString(string)` | `PromptResult` | Prompts for string input |
| `GetInteger(string)` | `PromptIntegerResult` | Prompts for integer |
| `GetDouble(string)` | `PromptDoubleResult` | Prompts for double |
| `GetPoint(string)` | `PromptPointResult` | Prompts for point |
| `GetDistance(string)` | `PromptDoubleResult` | Prompts for distance |
| `GetAngle(string)` | `PromptDoubleResult` | Prompts for angle |
| `GetKeywords(string, params string[])` | `PromptResult` | Prompts for keyword |

## Key Methods - Selection

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetSelection()` | `PromptSelectionResult` | Prompts for entity selection |
| `GetSelection(SelectionFilter)` | `PromptSelectionResult` | Selection with filter |
| `SelectAll(SelectionFilter)` | `PromptSelectionResult` | Selects all entities |

## Code Examples

### Example 1: Writing to Command Line
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
Editor ed = acDoc.Editor;

ed.WriteMessage("\nHello from AutoCAD!");
ed.WriteMessage("\nFormatted: {0}, {1}", 123, "test");
```

### Example 2: Getting User Input
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

// Get string
PromptStringOptions pso = new PromptStringOptions("\nEnter your name: ");
PromptResult pr = ed.GetString(pso);
if (pr.Status == PromptStatus.OK)
{
    string name = pr.StringResult;
    ed.WriteMessage($"\nHello, {name}!");
}

// Get integer
PromptIntegerOptions pio = new PromptIntegerOptions("\nEnter a number: ");
pio.AllowNegative = false;
PromptIntegerResult pir = ed.GetInteger(pio);
if (pir.Status == PromptStatus.OK)
{
    int number = pir.Value;
}

// Get point
PromptPointOptions ppo = new PromptPointOptions("\nSelect a point: ");
PromptPointResult ppr = ed.GetPoint(ppo);
if (ppr.Status == PromptStatus.OK)
{
    Point3d point = ppr.Value;
}
```

### Example 3: Entity Selection
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionResult selResult = ed.GetSelection();

if (selResult.Status == PromptStatus.OK)
{
    SelectionSet selSet = selResult.Value;
    ed.WriteMessage($"\n{selSet.Count} objects selected");
    
    ObjectId[] ids = selSet.GetObjectIds();
    // Work with selected objects
}
```

### Example 4: Filtered Selection
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

// Create filter for lines only
TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "LINE")
};
SelectionFilter filter = new SelectionFilter(filterList);

PromptSelectionResult selResult = ed.GetSelection(filter);

if (selResult.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{selResult.Value.Count} lines selected");
}
```

### Example 5: Keyword Input
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptKeywordOptions pko = new PromptKeywordOptions("\nChoose option [Yes/No]: ");
pko.Keywords.Add("Yes");
pko.Keywords.Add("No");
pko.Keywords.Default = "Yes";

PromptResult pr = ed.GetKeyword(pko);

if (pr.Status == PromptStatus.OK)
{
    string keyword = pr.StringResult;
    ed.WriteMessage($"\nYou chose: {keyword}");
}
```

## PromptStatus Values

| Status | Description |
|--------|-------------|
| `OK` | User provided valid input |
| `Cancel` | User cancelled (ESC) |
| `Error` | Error occurred |
| `None` | No input |
| `Keyword` | Keyword was entered |

## Related Objects
- [Document](Document.md) - Contains Editor
- [SelectionSet](SelectionSet.md) - Selected entities
- [SelectionFilter](SelectionFilter.md) - Entity filtering

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_EditorInput_Editor)
