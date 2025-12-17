# ErrorStatus Enumeration

## Overview
The `ErrorStatus` enumeration defines error codes returned by AutoCAD .NET API operations. Essential for error handling and debugging.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Common Values

| Value | Description |
|-------|-------------|
| `OK` | Operation succeeded |
| `NullObjectId` | Object ID is null |
| `InvalidInput` | Invalid input provided |
| `WrongObjectType` | Object is wrong type |
| `NotOpenForRead` | Object not open for read |
| `NotOpenForWrite` | Object not open for write |
| `ObjectToBeDeleted` | Object marked for deletion |
| `WasErased` | Object was erased |
| `InvalidOpenState` | Invalid open state |
| `WrongDatabase` | Object from wrong database |

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.DatabaseServices;

try
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
        tr.Commit();
    }
}
catch (Autodesk.AutoCAD.Runtime.Exception ex)
{
    if (ex.ErrorStatus == ErrorStatus.NullObjectId)
    {
        ed.WriteMessage("\nObject ID is null");
    }
    else if (ex.ErrorStatus == ErrorStatus.WasErased)
    {
        ed.WriteMessage("\nObject was erased");
    }
}
```

## Related Classes
- **Exception** - AutoCAD exception class
- **AcRx** - Runtime utilities

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
