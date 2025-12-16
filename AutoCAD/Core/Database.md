# Database Class

## Overview
The `Database` class represents an AutoCAD drawing database. It is the central repository for all graphical and non-graphical objects in a drawing, including entities, symbol tables, and dictionaries.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Database
```

## Key Properties

### Symbol Table Properties

| Property | Type | Description |
|----------|------|-------------|
| `BlockTableId` | `ObjectId` | Gets the ObjectId of the BlockTable |
| `LayerTableId` | `ObjectId` | Gets the ObjectId of the LayerTable |
| `LinetypeTableId` | `ObjectId` | Gets the ObjectId of the LinetypeTable |
| `TextStyleTableId` | `ObjectId` | Gets the ObjectId of the TextStyleTable |
| `DimStyleTableId` | `ObjectId` | Gets the ObjectId of the DimStyleTable |
| `UcsTableId` | `ObjectId` | Gets the ObjectId of the UcsTable |
| `ViewTableId` | `ObjectId` | Gets the ObjectId of the ViewTable |
| `ViewportTableId` | `ObjectId` | Gets the ObjectId of the ViewportTable |
| `RegAppTableId` | `ObjectId` | Gets the ObjectId of the RegAppTable |

### Dictionary Properties

| Property | Type | Description |
|----------|------|-------------|
| `NamedObjectsDictionaryId` | `ObjectId` | Gets the Named Objects Dictionary (NOD) |
| `GroupDictionaryId` | `ObjectId` | Gets the Group Dictionary |
| `MLStyleDictionaryId` | `ObjectId` | Gets the Multiline Style Dictionary |
| `LayoutDictionaryId` | `ObjectId` | Gets the Layout Dictionary |
| `PlotSettingsDictionaryId` | `ObjectId` | Gets the Plot Settings Dictionary |
| `ColorDictionaryId` | `ObjectId` | Gets the Color Dictionary |
| `MaterialDictionaryId` | `ObjectId` | Gets the Material Dictionary |
| `VisualStyleDictionaryId` | `ObjectId` | Gets the Visual Style Dictionary |

### Current State Properties

| Property | Type | Description |
|----------|------|-------------|
| `CurrentSpaceId` | `ObjectId` | Gets the ObjectId of the current space (Model or Paper) |
| `Clayer` | `ObjectId` | Gets/sets the current layer |
| `Celtype` | `ObjectId` | Gets/sets the current linetype |
| `Dimstyle` | `ObjectId` | Gets/sets the current dimension style |
| `Textstyle` | `ObjectId` | Gets/sets the current text style |
| `Cmaterial` | `ObjectId` | Gets/sets the current material |

### Drawing Properties

| Property | Type | Description |
|----------|------|-------------|
| `TileMode` | `bool` | Gets/sets whether TileMode is active (Model Space) |
| `Filename` | `string` | Gets the full path of the drawing file |
| `OriginalFileName` | `string` | Gets the original filename before any saves |
| `DwgVersion` | `DwgVersion` | Gets the DWG file version |
| `LastSavedAsVersion` | `DwgVersion` | Gets the version last saved as |

### System Properties

| Property | Type | Description |
|----------|------|-------------|
| `TransactionManager` | `TransactionManager` | Gets the transaction manager for this database |
| `ObjectContextManager` | `ObjectContextManager` | Gets the object context manager |
| `Dimscale` | `double` | Gets/sets the dimension scale |
| `Ltscale` | `double` | Gets/sets the linetype scale |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `ReadDwgFile(string, FileOpenMode, bool, string)` | `Database` | Reads a DWG file into a new Database object |
| `SaveAs(string, DwgVersion)` | `void` | Saves the database to a file |
| `Audit(bool, bool)` | `void` | Audits the database for errors |
| `Purge(ObjectIdCollection)` | `void` | Purges unused objects |
| `GetObjectId(bool, Handle, int)` | `ObjectId` | Gets an ObjectId from a handle |
| `GetAcadDatabase()` | `IntPtr` | Gets a pointer to the underlying AcDbDatabase |

## Code Examples

### Example 1: Accessing Symbol Tables
```csharp
Document doc = Application.DocumentManager.MdiActiveDocument;
Database db = doc.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Access BlockTable
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Access LayerTable
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    // Access current space
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    tr.Commit();
}
```

### Example 2: Reading an External DWG File
```csharp
using (Database extDb = new Database(false, true))
{
    extDb.ReadDwgFile("C:\\Drawings\\Sample.dwg", FileOpenMode.OpenForReadAndAllShare, true, "");
    
    using (Transaction tr = extDb.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(extDb.BlockTableId, OpenMode.ForRead) as BlockTable;
        // Work with external database
        
        tr.Commit();
    }
}
```

### Example 3: Getting Current Layer
```csharp
Database db = Application.DocumentManager.MdiActiveDocument.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTableRecord currentLayer = tr.GetObject(db.Clayer, OpenMode.ForRead) as LayerTableRecord;
    
    Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
    ed.WriteMessage($"\nCurrent layer: {currentLayer.Name}");
    
    tr.Commit();
}
```

### Example 4: Purging Unused Objects
```csharp
Database db = Application.DocumentManager.MdiActiveDocument.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectIdCollection idsToPurge = new ObjectIdCollection();
    
    // Get purgeable objects
    db.Purge(idsToPurge);
    
    if (idsToPurge.Count > 0)
    {
        // Purge the objects
        foreach (ObjectId id in idsToPurge)
        {
            DBObject obj = tr.GetObject(id, OpenMode.ForWrite);
            obj.Erase();
        }
    }
    
    tr.Commit();
}
```

## Related Objects
- [Document](Document.md) - Contains the Database
- [TransactionManager](TransactionManager.md) - Manages transactions for the database
- [BlockTable](BlockTable.md) - Symbol table for blocks
- [LayerTable](LayerTable.md) - Symbol table for layers
- [Entity](Entity.md) - Base class for graphical objects

### Example 5: Accessing Named Objects Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get the Named Objects Dictionary (NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\nNamed Objects Dictionary entries:");
    foreach (DBDictionaryEntry entry in nod)
    {
        ed.WriteMessage($"\n  {entry.Key}");
    }
    
    tr.Commit();
}
```

### Example 6: Accessing Group Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage($"\nNumber of groups: {groupDict.Count}");
    
    foreach (DBDictionaryEntry entry in groupDict)
    {
        Group group = tr.GetObject(entry.Value, OpenMode.ForRead) as Group;
        ed.WriteMessage($"\n  Group: {entry.Key} - {group.Count} objects");
    }
    
    tr.Commit();
}
```

### Example 7: Accessing Layout Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\nLayouts in drawing:");
    foreach (DBDictionaryEntry entry in layoutDict)
    {
        Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
        ed.WriteMessage($"\n  {layout.LayoutName}");
    }
    
    tr.Commit();
}
```

### Example 8: Creating Entry in Named Objects Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Create a custom dictionary
    string dictName = "MyCustomDictionary";
    
    if (!nod.Contains(dictName))
    {
        DBDictionary customDict = new DBDictionary();
        nod.SetAt(dictName, customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
        
        ed.WriteMessage($"\nCreated custom dictionary: {dictName}");
    }
    
    tr.Commit();
}
```

### Working with TileMode (Model/Paper Space)
```csharp
Database db = Application.DocumentManager.MdiActiveDocument.Database;

if (db.TileMode)
{
    // Currently in Model Space
}
else
{
    // Currently in Paper Space
}
```

### Accessing Model Space and Paper Space
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Model Space
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Paper Space
    BlockTableRecord paperSpace = tr.GetObject(bt[BlockTableRecord.PaperSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    tr.Commit();
}
```

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-C8C7E74A-ACAD-4E1E-B90E-2C9F6D1E7C3E)
- [Database Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Database)
