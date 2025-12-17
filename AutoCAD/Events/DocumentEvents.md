# Document Events

**Namespace:** `Autodesk.AutoCAD.ApplicationServices`  
**Assembly:** `acmgd.dll`

## Overview

The `Document` class exposes events that fire during document lifecycle operations such as activation, deactivation, command execution, and window state changes. These events enable you to respond to user interactions and document state changes.

**Key Concept:** Document events track the lifecycle and state of individual drawing documents, allowing you to execute code when documents are opened, closed, activated, or when commands are executed.

## Key Events

| Event | Description |
|-------|-------------|
| `CommandWillStart` | Fires before a command starts |
| `CommandEnded` | Fires after a command ends |
| `CommandCancelled` | Fires when a command is cancelled |
| `CommandFailed` | Fires when a command fails |
| `LispWillStart` | Fires before a LISP expression evaluates |
| `LispEnded` | Fires after a LISP expression completes |
| `LispCancelled` | Fires when LISP evaluation is cancelled |
| `BeginDocumentClose` | Fires before document closes |
| `DocumentLockModeWillChange` | Fires before document lock mode changes |
| `DocumentLockModeChanged` | Fires after document lock mode changes |
| `ImpliedSelectionChanged` | Fires when pickfirst selection changes |

## Common Usage Patterns

### 1. Monitoring Command Execution

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Runtime;

public class CommandMonitor
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        
        _doc.CommandWillStart += OnCommandWillStart;
        _doc.CommandEnded += OnCommandEnded;
        _doc.CommandCancelled += OnCommandCancelled;
        _doc.CommandFailed += OnCommandFailed;
    }
    
    public void Stop()
    {
        if (_doc != null)
        {
            _doc.CommandWillStart -= OnCommandWillStart;
            _doc.CommandEnded -= OnCommandEnded;
            _doc.CommandCancelled -= OnCommandCancelled;
            _doc.CommandFailed -= OnCommandFailed;
        }
    }
    
    private void OnCommandWillStart(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Command Starting: {e.GlobalCommandName}");
    }
    
    private void OnCommandEnded(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Command Ended: {e.GlobalCommandName}");
    }
    
    private void OnCommandCancelled(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Command Cancelled: {e.GlobalCommandName}");
    }
    
    private void OnCommandFailed(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Command Failed: {e.GlobalCommandName}");
    }
}

[CommandMethod("STARTCMDMON")]
public void StartCommandMonitoring()
{
    CommandMonitor monitor = new CommandMonitor();
    monitor.Start();
    
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage(
        "\nCommand monitoring started");
}
```

### 2. Command Timing and Performance Tracking

```csharp
public class CommandPerformanceTracker
{
    private Document _doc;
    private System.Diagnostics.Stopwatch _stopwatch;
    private string _currentCommand;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        _stopwatch = new System.Diagnostics.Stopwatch();
        
        _doc.CommandWillStart += OnCommandWillStart;
        _doc.CommandEnded += OnCommandEnded;
    }
    
    private void OnCommandWillStart(object sender, CommandEventArgs e)
    {
        _currentCommand = e.GlobalCommandName;
        _stopwatch.Restart();
    }
    
    private void OnCommandEnded(object sender, CommandEventArgs e)
    {
        _stopwatch.Stop();
        double seconds = _stopwatch.Elapsed.TotalSeconds;
        
        _doc.Editor.WriteMessage(
            $"\n>>> {_currentCommand} completed in {seconds:F3} seconds");
    }
}
```

### 3. Document Close Prevention

```csharp
public class CloseGuard
{
    private Document _doc;
    private bool _allowClose = false;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        _doc.BeginDocumentClose += OnBeginDocumentClose;
    }
    
    private void OnBeginDocumentClose(object sender, DocumentBeginCloseEventArgs e)
    {
        if (!_allowClose)
        {
            // Check if there are unsaved custom data
            bool hasUnsavedData = CheckForUnsavedData();
            
            if (hasUnsavedData)
            {
                System.Windows.Forms.DialogResult result = 
                    System.Windows.Forms.MessageBox.Show(
                        "You have unsaved custom data. Close anyway?",
                        "Warning",
                        System.Windows.Forms.MessageBoxButtons.YesNo,
                        System.Windows.Forms.MessageBoxIcon.Warning);
                
                if (result == System.Windows.Forms.DialogResult.No)
                {
                    e.Veto(); // Prevent close
                }
            }
        }
    }
    
    private bool CheckForUnsavedData()
    {
        // Your custom logic here
        return false;
    }
}
```

### 4. LISP Expression Monitoring

```csharp
public class LispMonitor
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        
        _doc.LispWillStart += OnLispWillStart;
        _doc.LispEnded += OnLispEnded;
        _doc.LispCancelled += OnLispCancelled;
    }
    
    private void OnLispWillStart(object sender, LispWillStartEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> LISP Starting: {e.FirstLine}");
    }
    
    private void OnLispEnded(object sender, LispEndedEventArgs e)
    {
        _doc.Editor.WriteMessage("\n>>> LISP Ended");
    }
    
    private void OnLispCancelled(object sender, EventArgs e)
    {
        _doc.Editor.WriteMessage("\n>>> LISP Cancelled");
    }
}
```

### 5. Document Lock Mode Tracking

```csharp
public class LockModeTracker
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        
        _doc.DocumentLockModeWillChange += OnLockModeWillChange;
        _doc.DocumentLockModeChanged += OnLockModeChanged;
    }
    
    private void OnLockModeWillChange(object sender, DocumentLockModeWillChangeEventArgs e)
    {
        _doc.Editor.WriteMessage(
            $"\n>>> Lock Mode Will Change: {e.CurrentMode} → {e.NewMode}");
        
        _doc.Editor.WriteMessage(
            $"\n    Global Command: {e.GlobalCommandName}");
    }
    
    private void OnLockModeChanged(object sender, DocumentLockModeChangedEventArgs e)
    {
        _doc.Editor.WriteMessage(
            $"\n>>> Lock Mode Changed: {e.CurrentMode}");
    }
}
```

### 6. Selection Change Monitoring

```csharp
public class SelectionMonitor
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        _doc.ImpliedSelectionChanged += OnImpliedSelectionChanged;
    }
    
    private void OnImpliedSelectionChanged(object sender, EventArgs e)
    {
        using (Transaction tr = _doc.Database.TransactionManager.StartTransaction())
        {
            PromptSelectionResult psr = _doc.Editor.SelectImplied();
            
            if (psr.Status == PromptStatus.OK)
            {
                _doc.Editor.WriteMessage(
                    $"\n>>> Selection Changed: {psr.Value.Count} objects selected");
            }
            else
            {
                _doc.Editor.WriteMessage("\n>>> Selection Cleared");
            }
            
            tr.Commit();
        }
    }
}
```

## Best Practices

1. **Document-specific events**: Register events per document, not globally
2. **Always unregister**: Remove event handlers when done
3. **Avoid heavy processing**: Keep event handlers lightweight
4. **Use Veto carefully**: Only veto operations when absolutely necessary
5. **Handle exceptions**: Wrap event code in try-catch blocks

## Common Patterns

### Pattern 1: Command Lifecycle
```csharp
_doc.CommandWillStart += OnStart;
_doc.CommandEnded += OnEnd;
_doc.CommandCancelled += OnCancel;
_doc.CommandFailed += OnFail;
```

### Pattern 2: Document Protection
```csharp
_doc.BeginDocumentClose += (s, e) => {
    if (NeedsSave()) e.Veto();
};
```

## Related Classes

- [Document](../Core/Document.md) - Exposes these events
- [DocumentCollection](../Core/DocumentManager.md) - Document collection events
- [CommandEventArgs](CommandEventArgs.md) - Command event arguments
- [Editor](../Core/Editor.md) - User interaction

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-DocumentEvents)
