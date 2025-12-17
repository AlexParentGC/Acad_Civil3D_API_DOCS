# CommandMethodAttribute Class

## Overview
The `CommandMethodAttribute` class is used to mark methods as AutoCAD commands. This attribute enables methods to be invoked from the AutoCAD command line.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Inheritance Hierarchy
```
System.Object
  └─ System.Attribute
      └─ CommandMethodAttribute
```

## Constructor

| Constructor | Description |
|-------------|-------------|
| `CommandMethodAttribute(string)` | Creates command with specified name |
| `CommandMethodAttribute(string, CommandFlags)` | Creates command with name and flags |

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `GlobalName` | `string` | Gets or sets the global command name |
| `LocalizedNameId` | `string` | Gets or sets the localized name resource ID |
| `GroupName` | `string` | Gets or sets the command group name |
| `Flags` | `CommandFlags` | Gets or sets the command flags |

## CommandFlags Enumeration

| Flag | Description |
|------|-------------|
| `Modal` | Command runs in application context (default) |
| `Transparent` | Command can run while another command is active |
| `UsePickSet` | Command uses the current pickfirst selection set |
| `Redraw` | Command causes a redraw |
| `NoPerspective` | Command cannot run in perspective view |
| `NoMultiple` | Command cannot be repeated with MULTIPLE command |
| `NoTileMode` | Command cannot run in tiled viewports |
| `NoPaperSpace` | Command cannot run in paper space |
| `NoOem` | Command not available to OEM applications |
| `Undefined` | Command is undefined |
| `Inprogress` | Command is in progress |
| `Defun` | Command is defined as a LISP function |
| `NoNewStack` | Command does not create a new undo stack |
| `NoInternalLock` | Command does not lock the document |
| `DocReadLock` | Command requires document read lock |
| `DocExclusiveLock` | Command requires document exclusive lock |
| `Session` | Command is session-wide |
| `Interruptible` | Command can be interrupted |
| `NoHistory` | Command is not added to command history |
| `NoUndoMarker` | Command does not create undo marker |
| `NoBlockEditor` | Command cannot run in block editor |
| `NoActionRecording` | Command is not recorded in action macros |
| `ActionMacro` | Command is an action macro |
| `NoInferConstraint` | Command does not infer constraints |
| `TempShowDynDimension` | Temporarily shows dynamic dimensions |

## Code Examples

### Example 1: Basic Command
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;

[CommandMethod("HelloWorld")]
public void HelloWorldCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    
    ed.WriteMessage("\nHello, AutoCAD World!");
}
```

### Example 2: Command with Flags
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;

[CommandMethod("MyCommand", CommandFlags.Modal)]
public void MyCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    
    ed.WriteMessage("\nThis is a modal command");
}
```

### Example 3: Transparent Command
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;

[CommandMethod("ZoomExtents", CommandFlags.Transparent)]
public void ZoomExtentsCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    
    // This command can run while another command is active
    doc.SendStringToExecute("._zoom _e ", true, false, false);
}
```

### Example 4: Command with PickFirst Selection
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.DatabaseServices;

[CommandMethod("ChangeColor", CommandFlags.UsePickSet)]
public void ChangeColorCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    Database db = doc.Database;
    
    // Get the pickfirst selection set
    PromptSelectionResult result = ed.SelectImplied();
    
    if (result.Status == PromptStatus.OK)
    {
        using (Transaction tr = db.TransactionManager.StartTransaction())
        {
            foreach (SelectedObject selObj in result.Value)
            {
                Entity ent = tr.GetObject(selObj.ObjectId, OpenMode.ForWrite) as Entity;
                ent.ColorIndex = 1; // Red
            }
            tr.Commit();
        }
        
        ed.WriteMessage($"\n{result.Value.Count} objects changed to red");
    }
    else
    {
        ed.WriteMessage("\nNo objects selected");
    }
}
```

### Example 5: Command with Document Locking
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

[CommandMethod("ModifyDrawing", CommandFlags.Modal | CommandFlags.DocExclusiveLock)]
public void ModifyDrawingCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // This command has exclusive lock on the document
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Safely modify database objects
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForWrite) as BlockTableRecord;
        
        // Add new entity
        Line line = new Line(new Point3d(0, 0, 0), new Point3d(100, 100, 0));
        btr.AppendEntity(line);
        tr.AddNewlyCreatedDBObject(line, true);
        
        tr.Commit();
    }
    
    ed.WriteMessage("\nDrawing modified successfully");
}
```

### Example 6: Command Group
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;

public class MyCommands
{
    [CommandMethod("MyGroup", "Command1", CommandFlags.Modal)]
    public void Command1()
    {
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nCommand 1");
    }
    
    [CommandMethod("MyGroup", "Command2", CommandFlags.Modal)]
    public void Command2()
    {
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nCommand 2");
    }
}
```

## Common Patterns

### Session Command
```csharp
[CommandMethod("SessionCommand", CommandFlags.Session)]
public void SessionCommand()
{
    // Runs at application level, not document level
}
```

### No Undo Command
```csharp
[CommandMethod("NoUndoCommand", CommandFlags.Modal | CommandFlags.NoUndoMarker)]
public void NoUndoCommand()
{
    // This command won't create an undo marker
}
```

## Best Practices

1. **Command Names**: Use clear, descriptive command names
2. **Flags**: Choose appropriate flags for command behavior
3. **Locking**: Use `DocReadLock` or `DocExclusiveLock` when modifying database
4. **Modal**: Most commands should be Modal
5. **Transparent**: Use sparingly for commands that don't modify the drawing
6. **PickFirst**: Use `UsePickSet` for commands that work with pre-selected objects
7. **Error Handling**: Always use try-catch for robust commands
8. **Transactions**: Use transactions for database modifications

## Related Classes
- **LispFunctionAttribute** - Expose methods to LISP
- **CommandClass** - Command registration
- **CommandFlags** - Command behavior flags
- **Editor** - Command line interaction
- **Document** - Active document

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [CommandMethodAttribute Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Runtime_CommandMethodAttribute)
