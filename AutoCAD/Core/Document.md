# Document Class

## Overview
The `Document` class represents an open AutoCAD drawing document. It provides access to the database, editor, and document-level operations.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Inheritance Hierarchy
```
System.Object
  └─ Document
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Database` | `Database` | Gets the document's database |
| `Editor` | `Editor` | Gets the document's editor |
| `Name` | `string` | Gets the document name (filename) |
| `Window` | `Window` | Gets the document window |
| `UserData` | `IDictionary` | Gets user-defined data dictionary |
| `IsReadOnly` | `bool` | Gets whether document is read-only |
| `IsActive` | `bool` | Gets whether document is active |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `LockDocument()` | `DocumentLock` | Locks the document for modifications |
| `LockDocument(DocumentLockMode, string, string, bool)` | `DocumentLock` | Locks with specific mode |
| `CloseAndDiscard()` | `void` | Closes document without saving |
| `CloseAndSave(string)` | `void` | Closes and saves document |
| `SendStringToExecute(string, bool, bool, bool)` | `void` | Executes a command string |

## Code Examples

### Example 1: Accessing Document Properties
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;

ed.WriteMessage($"\nDocument Name: {acDoc.Name}");
ed.WriteMessage($"\nIs Active: {acDoc.IsActive}");
ed.WriteMessage($"\nIs Read-Only: {acDoc.IsReadOnly}");

Database db = acDoc.Database;
Editor ed = acDoc.Editor;
```

### Example 2: Locking Document for Modifications
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
Database db = acDoc.Database;

// Lock document before modifying
using (DocumentLock docLock = acDoc.LockDocument())
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Modify database objects here
        
        tr.Commit();
    }
}
```

### Example 3: Executing Commands
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;

// Execute a command
acDoc.SendStringToExecute("ZOOM E ", true, false, false);
```

### Example 4: Saving Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
Database db = acDoc.Database;

// Save current document
db.SaveAs(acDoc.Name, DwgVersion.Current);

// Or save with new name
db.SaveAs("C:\\Drawings\\NewName.dwg", DwgVersion.Current);
```

### Example 5: Using User Data
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;

// Store custom data with document
acDoc.UserData["MyKey"] = "MyValue";

// Retrieve custom data
if (acDoc.UserData.Contains("MyKey"))
{
    string value = acDoc.UserData["MyKey"] as string;
    ed.WriteMessage($"\nStored value: {value}");
}
```

## Document Lock Modes

| Mode | Description |
|------|-------------|
| `NotLocked` | Document is not locked |
| `AutoWrite` | Automatic write lock |
| `ProtectedAutoWrite` | Protected automatic write lock |
| `Read` | Read-only lock |
| `Write` | Write lock |

## Related Objects
- [Application](Application.md) - Root application object
- [DocumentManager](DocumentManager.md) - Manages documents
- [Database](Database.md) - Document's database
- [Editor](Editor.md) - User interaction

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_Document)
