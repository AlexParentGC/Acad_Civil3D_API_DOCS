# Application Events

**Namespace:** `Autodesk.AutoCAD.ApplicationServices`  
**Assembly:** `acmgd.dll`

## Overview

The `Application` class exposes application-level events that fire when documents are created, opened, closed, or when the application state changes. These are global events that apply across all documents.

**Key Concept:** Application events monitor the AutoCAD application itself, not individual documents. Use these for application-wide operations like managing document collections or responding to system-level changes.

## Key Events

| Event | Description |
|-------|-------------|
| `DocumentCreated` | Fires when a new document is created |
| `DocumentToBeDestroyed` | Fires before a document is destroyed |
| `DocumentDestroyed` | Fires after a document is destroyed |
| `DocumentActivated` | Fires when a document becomes active |
| `DocumentToBeActivated` | Fires before a document becomes active |
| `DocumentToBeDeactivated` | Fires before a document is deactivated |
| `DocumentBecameCurrent` | Fires when a document becomes current |
| `BeginQuit` | Fires before AutoCAD quits |
| `QuitAborted` | Fires when quit is aborted |
| `QuitWillStart` | Fires when quit will start |
| `SystemVariableChanged` | Fires when a system variable changes |
| `SystemVariableWillChange` | Fires before a system variable changes |

## Common Usage Patterns

### 1. Document Lifecycle Monitoring

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Runtime;

public class ApplicationEventMonitor
{
    public void RegisterEvents()
    {
        Application.DocumentManager.DocumentCreated += OnDocumentCreated;
        Application.DocumentManager.DocumentToBeDestroyed += OnDocumentToBeDestroyed;
        Application.DocumentManager.DocumentDestroyed += OnDocumentDestroyed;
    }
    
    public void UnregisterEvents()
    {
        Application.DocumentManager.DocumentCreated -= OnDocumentCreated;
        Application.DocumentManager.DocumentToBeDestroyed -= OnDocumentToBeDestroyed;
        Application.DocumentManager.DocumentDestroyed -= OnDocumentDestroyed;
    }
    
    private void OnDocumentCreated(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Document Created: {e.Document.Name}");
    }
    
    private void OnDocumentToBeDestroyed(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Document Will Be Destroyed: {e.Document.Name}");
    }
    
    private void OnDocumentDestroyed(object sender, DocumentDestroyedEventArgs e)
    {
        // Note: Document is already destroyed, can't access Editor
        System.Diagnostics.Debug.WriteLine($"Document Destroyed: {e.FileName}");
    }
}

[CommandMethod("STARTAPPMON")]
public void StartApplicationMonitoring()
{
    ApplicationEventMonitor monitor = new ApplicationEventMonitor();
    monitor.RegisterEvents();
    
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage(
        "\nApplication event monitoring started");
}
```

### 2. Document Activation Tracking

```csharp
public class DocumentActivationTracker
{
    private int _activationCount = 0;
    
    public void Start()
    {
        Application.DocumentManager.DocumentActivated += OnDocumentActivated;
        Application.DocumentManager.DocumentToBeActivated += OnDocumentToBeActivated;
        Application.DocumentManager.DocumentToBeDeactivated += OnDocumentToBeDeactivated;
    }
    
    private void OnDocumentToBeActivated(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Will Activate: {e.Document.Name}");
    }
    
    private void OnDocumentActivated(object sender, DocumentCollectionEventArgs e)
    {
        _activationCount++;
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Activated: {e.Document.Name} (Count: {_activationCount})");
    }
    
    private void OnDocumentToBeDeactivated(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Will Deactivate: {e.Document.Name}");
    }
}
```

### 3. Auto-Save on Document Switch

```csharp
public class AutoSaveOnSwitch
{
    public void Start()
    {
        Application.DocumentManager.DocumentToBeDeactivated += OnDocumentToBeDeactivated;
    }
    
    private void OnDocumentToBeDeactivated(object sender, DocumentCollectionEventArgs e)
    {
        Document doc = e.Document;
        
        // Check if document has unsaved changes
        if (doc.Database.HasSaveVersionInfo)
        {
            try
            {
                // Auto-save before switching
                doc.Database.SaveAs(doc.Name, true, DwgVersion.Current, doc.Database.SecurityParameters);
                doc.Editor.WriteMessage("\n>>> Auto-saved before switching documents");
            }
            catch (System.Exception ex)
            {
                doc.Editor.WriteMessage($"\n>>> Auto-save failed: {ex.Message}");
            }
        }
    }
}
```

### 4. System Variable Change Monitoring

```csharp
public class SysVarMonitor
{
    public void Start()
    {
        Application.SystemVariableWillChange += OnSysVarWillChange;
        Application.SystemVariableChanged += OnSysVarChanged;
    }
    
    private void OnSysVarWillChange(object sender, SystemVariableChangingEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> SysVar Will Change: {e.Name}");
        ed.WriteMessage($"\n    Current Value: {Application.GetSystemVariable(e.Name)}");
    }
    
    private void OnSysVarChanged(object sender, SystemVariableChangedEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> SysVar Changed: {e.Name}");
        ed.WriteMessage($"\n    New Value: {Application.GetSystemVariable(e.Name)}");
    }
}
```

### 5. Application Quit Handler

```csharp
public class QuitHandler
{
    public void Start()
    {
        Application.QuitWillStart += OnQuitWillStart;
        Application.QuitAborted += OnQuitAborted;
        Application.BeginQuit += OnBeginQuit;
    }
    
    private void OnBeginQuit(object sender, EventArgs e)
    {
        System.Diagnostics.Debug.WriteLine(">>> AutoCAD is beginning to quit");
        
        // Perform cleanup operations
        CleanupResources();
    }
    
    private void OnQuitWillStart(object sender, EventArgs e)
    {
        System.Diagnostics.Debug.WriteLine(">>> AutoCAD quit will start");
    }
    
    private void OnQuitAborted(object sender, EventArgs e)
    {
        System.Diagnostics.Debug.WriteLine(">>> AutoCAD quit was aborted");
    }
    
    private void CleanupResources()
    {
        // Close log files, save settings, etc.
    }
}
```

### 6. Multi-Document Statistics

```csharp
public class DocumentStatistics
{
    private int _totalCreated = 0;
    private int _totalDestroyed = 0;
    private int _currentOpen = 0;
    
    public void Start()
    {
        Application.DocumentManager.DocumentCreated += OnDocumentCreated;
        Application.DocumentManager.DocumentDestroyed += OnDocumentDestroyed;
    }
    
    private void OnDocumentCreated(object sender, DocumentCollectionEventArgs e)
    {
        _totalCreated++;
        _currentOpen++;
        
        e.Document.Editor.WriteMessage("\n╔════════════════════════════════╗");
        e.Document.Editor.WriteMessage("\n║   DOCUMENT STATISTICS          ║");
        e.Document.Editor.WriteMessage("\n╠════════════════════════════════╣");
        e.Document.Editor.WriteMessage($"\n║ Total Created:  {_totalCreated,14} ║");
        e.Document.Editor.WriteMessage($"\n║ Total Destroyed: {_totalDestroyed,13} ║");
        e.Document.Editor.WriteMessage($"\n║ Currently Open:  {_currentOpen,13} ║");
        e.Document.Editor.WriteMessage("\n╚════════════════════════════════╝");
    }
    
    private void OnDocumentDestroyed(object sender, DocumentDestroyedEventArgs e)
    {
        _totalDestroyed++;
        _currentOpen--;
        
        System.Diagnostics.Debug.WriteLine($"Document destroyed. Current open: {_currentOpen}");
    }
}
```

## Best Practices

1. **Application-wide only**: Use for cross-document operations
2. **Always unregister**: Critical for application events
3. **Minimal processing**: Keep handlers very fast
4. **Thread-safe**: Application events may fire on different threads
5. **Careful with Document access**: Some events fire when documents are unavailable

## Common Patterns

### Pattern 1: Document Lifecycle
```csharp
Application.DocumentManager.DocumentCreated += OnCreated;
Application.DocumentManager.DocumentDestroyed += OnDestroyed;
```

### Pattern 2: Activation Tracking
```csharp
Application.DocumentManager.DocumentActivated += OnActivated;
Application.DocumentManager.DocumentToBeDeactivated += OnDeactivated;
```

### Pattern 3: Application Cleanup
```csharp
Application.BeginQuit += (s, e) => {
    // Cleanup resources
};
```

## Related Classes

- [Application](../Core/Application.md) - Exposes these events
- [DocumentManager](../Core/DocumentManager.md) - Document collection manager
- [Document](../Core/Document.md) - Individual document
- [DocumentCollectionEventArgs](DocumentCollectionEventArgs.md) - Event arguments

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-ApplicationEvents)
