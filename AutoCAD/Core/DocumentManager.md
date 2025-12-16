# DocumentManager Class

## Overview
The `DocumentManager` class manages all open documents in AutoCAD. It provides access to the active document and allows iteration through all open documents.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Inheritance Hierarchy
```
System.Object
  └─ DocumentManager
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `MdiActiveDocument` | `Document` | Gets the currently active document |
| `CurrentDocument` | `Document` | Gets the current document context |
| `Count` | `int` | Gets the number of open documents |

## Key Events

| Event | Description |
|-------|-------------|
| `DocumentCreated` | Fired when a new document is created |
| `DocumentToBeDestroyed` | Fired before a document is closed |
| `DocumentActivated` | Fired when a document becomes active |
| `DocumentBecameCurrent` | Fired when a document becomes current |

## Code Examples

### Example 1: Accessing Active Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;

DocumentManager docMgr = Application.DocumentManager;
Document acDoc = docMgr.MdiActiveDocument;

if (acDoc != null)
{
    Editor ed = acDoc.Editor;
    ed.WriteMessage($"\nActive document: {acDoc.Name}");
}
```

### Example 2: Iterating Through All Documents
```csharp
using Autodesk.AutoCAD.ApplicationServices;

DocumentManager docMgr = Application.DocumentManager;

ed.WriteMessage($"\nTotal open documents: {docMgr.Count}");

foreach (Document doc in docMgr)
{
    ed.WriteMessage($"\n  {doc.Name}");
}
```

### Example 3: Creating a New Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;

DocumentManager docMgr = Application.DocumentManager;

// Create new document from template
Document newDoc = docMgr.Add("acad.dwt");

// Make it active
docMgr.MdiActiveDocument = newDoc;
```

### Example 4: Handling Document Events
```csharp
using Autodesk.AutoCAD.ApplicationServices;

public void RegisterDocumentEvents()
{
    DocumentManager docMgr = Application.DocumentManager;
    
    docMgr.DocumentCreated += OnDocumentCreated;
    docMgr.DocumentToBeDestroyed += OnDocumentToBeDestroyed;
}

private void OnDocumentCreated(object sender, DocumentCollectionEventArgs e)
{
    Document doc = e.Document;
    Editor ed = doc.Editor;
    ed.WriteMessage($"\nDocument created: {doc.Name}");
}

private void OnDocumentToBeDestroyed(object sender, DocumentCollectionEventArgs e)
{
    Document doc = e.Document;
    // Cleanup before document closes
}
```

## Related Objects
- [Application](Application.md) - Provides access to DocumentManager
- [Document](Document.md) - Individual document
- [Database](Database.md) - Document's database

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_DocumentManager)
