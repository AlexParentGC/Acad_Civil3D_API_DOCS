# DBDictionary Class

## Overview
The `DBDictionary` class is a container object that stores named entries of other database objects. Dictionaries provide a hierarchical structure for organizing custom data, settings, and objects in an AutoCAD drawing. The most important dictionary is the Named Objects Dictionary (NOD), accessible from the Database.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ DBDictionary
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Count` | `int` | Gets the number of entries in the dictionary |
| `TreatElementsAsHard` | `bool` | Gets/sets whether dictionary entries are hard-owned or soft-owned |
| `IsClonable` | `bool` | Gets whether the dictionary can be cloned |
| `MergeStyle` | `DictionaryMergeStyle` | Gets/sets how the dictionary is merged during WBLOCK operations |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SetAt(string, DBObject)` | `ObjectId` | Adds or replaces an entry in the dictionary |
| `GetAt(string)` | `ObjectId` | Gets the ObjectId of an entry by key name |
| `Contains(string)` | `bool` | Checks if a key exists in the dictionary |
| `Remove(string)` | `void` | Removes an entry from the dictionary |
| `GetEnumerator()` | `IEnumerator` | Gets an enumerator to iterate through entries |

## Code Examples

### Example 1: Accessing the Named Objects Dictionary
```csharp
Document doc = Application.DocumentManager.MdiActiveDocument;
Database db = doc.Database;
Editor ed = doc.Editor;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get the Named Objects Dictionary (NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage($"\n=== Named Objects Dictionary ===");
    ed.WriteMessage($"\nTotal entries: {nod.Count}");
    ed.WriteMessage($"\nTreat elements as hard: {nod.TreatElementsAsHard}");
    
    // List all top-level entries
    ed.WriteMessage("\n\nTop-level entries:");
    foreach (DBDictionaryEntry entry in nod)
    {
        DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
        ed.WriteMessage($"\n  {entry.Key} ({obj.GetType().Name})");
    }
    
    tr.Commit();
}
```

### Example 2: Creating a Custom Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    string dictName = "MyCompanyData";
    
    // Check if dictionary already exists
    if (!nod.Contains(dictName))
    {
        // Create new dictionary
        DBDictionary customDict = new DBDictionary();
        
        // Add to NOD
        ObjectId dictId = nod.SetAt(dictName, customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
        
        ed.WriteMessage($"\nCreated custom dictionary: {dictName}");
        ed.WriteMessage($"\nDictionary ObjectId: {dictId}");
    }
    else
    {
        ed.WriteMessage($"\nDictionary '{dictName}' already exists");
    }
    
    tr.Commit();
}
```

### Example 3: Adding Entries to a Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    // Get or create custom dictionary
    DBDictionary customDict;
    if (nod.Contains("MyCompanyData"))
    {
        customDict = tr.GetObject(nod.GetAt("MyCompanyData"), OpenMode.ForWrite) as DBDictionary;
    }
    else
    {
        nod.UpgradeOpen();
        customDict = new DBDictionary();
        nod.SetAt("MyCompanyData", customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
    }
    
    // Create and add XRecords
    string[] recordNames = { "ProjectInfo", "Settings", "Metadata" };
    
    foreach (string recordName in recordNames)
    {
        if (!customDict.Contains(recordName))
        {
            XRecord xRec = new XRecord();
            ResultBuffer rb = new ResultBuffer(
                new TypedValue((int)DxfCode.Text, recordName),
                new TypedValue((int)DxfCode.Text, DateTime.Now.ToString()),
                new TypedValue((int)DxfCode.Int32, 1)
            );
            xRec.Data = rb;
            
            customDict.SetAt(recordName, xRec);
            tr.AddNewlyCreatedDBObject(xRec, true);
            
            rb.Dispose();
            
            ed.WriteMessage($"\nAdded entry: {recordName}");
        }
    }
    
    tr.Commit();
}
```

### Example 4: Enumerating Dictionary Contents
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyCompanyData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyCompanyData"), OpenMode.ForRead) as DBDictionary;
        
        ed.WriteMessage($"\n=== MyCompanyData Dictionary ===");
        ed.WriteMessage($"\nEntries: {customDict.Count}");
        
        // Method 1: Using foreach with DBDictionaryEntry
        foreach (DBDictionaryEntry entry in customDict)
        {
            DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
            ed.WriteMessage($"\n  Key: {entry.Key}");
            ed.WriteMessage($"\n    Type: {obj.GetType().Name}");
            ed.WriteMessage($"\n    ObjectId: {entry.Value}");
            
            if (obj is XRecord)
            {
                XRecord xRec = obj as XRecord;
                ResultBuffer rb = xRec.Data;
                if (rb != null)
                {
                    ed.WriteMessage($"\n    Data items: {rb.AsArray().Length}");
                    rb.Dispose();
                }
            }
        }
    }
    
    tr.Commit();
}
```

### Example 5: Creating Nested Dictionaries
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Create main dictionary
    DBDictionary mainDict = new DBDictionary();
    nod.SetAt("ProjectData", mainDict);
    tr.AddNewlyCreatedDBObject(mainDict, true);
    
    // Create nested dictionaries
    string[] categories = { "Structural", "Mechanical", "Electrical" };
    
    foreach (string category in categories)
    {
        DBDictionary categoryDict = new DBDictionary();
        mainDict.SetAt(category, categoryDict);
        tr.AddNewlyCreatedDBObject(categoryDict, true);
        
        // Add XRecords to each category
        XRecord xRec = new XRecord();
        ResultBuffer rb = new ResultBuffer(
            new TypedValue((int)DxfCode.Text, $"{category} Data"),
            new TypedValue((int)DxfCode.Real, 100.0)
        );
        xRec.Data = rb;
        
        categoryDict.SetAt("Info", xRec);
        tr.AddNewlyCreatedDBObject(xRec, true);
        
        rb.Dispose();
        
        ed.WriteMessage($"\nCreated category: {category}");
    }
    
    tr.Commit();
}
```

### Example 6: Working with Standard Dictionaries
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Access Group Dictionary
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, OpenMode.ForRead) as DBDictionary;
    ed.WriteMessage($"\n=== Groups ===");
    ed.WriteMessage($"\nTotal groups: {groupDict.Count}");
    
    foreach (DBDictionaryEntry entry in groupDict)
    {
        Group group = tr.GetObject(entry.Value, OpenMode.ForRead) as Group;
        ed.WriteMessage($"\n  {entry.Key}: {group.Count} objects");
    }
    
    // Access Layout Dictionary
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    ed.WriteMessage($"\n\n=== Layouts ===");
    ed.WriteMessage($"\nTotal layouts: {layoutDict.Count}");
    
    foreach (DBDictionaryEntry entry in layoutDict)
    {
        Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
        ed.WriteMessage($"\n  {layout.LayoutName} (Model: {layout.ModelType})");
    }
    
    // Access Material Dictionary
    DBDictionary materialDict = tr.GetObject(db.MaterialDictionaryId, OpenMode.ForRead) as DBDictionary;
    ed.WriteMessage($"\n\n=== Materials ===");
    ed.WriteMessage($"\nTotal materials: {materialDict.Count}");
    
    tr.Commit();
}
```

### Example 7: Working with Entity Extension Dictionaries
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get an entity
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Create extension dictionary if it doesn't exist
    if (ent.ExtensionDictionary == ObjectId.Null)
    {
        ent.CreateExtensionDictionary();
        ed.WriteMessage("\nCreated extension dictionary");
    }
    
    // Access the extension dictionary
    DBDictionary extDict = tr.GetObject(ent.ExtensionDictionary, OpenMode.ForWrite) as DBDictionary;
    
    // Add custom data
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "EntityMetadata"),
        new TypedValue((int)DxfCode.Text, "Author"),
        new TypedValue((int)DxfCode.Text, "John Doe"),
        new TypedValue((int)DxfCode.Text, "Date"),
        new TypedValue((int)DxfCode.Text, DateTime.Now.ToString("yyyy-MM-dd"))
    );
    xRec.Data = rb;
    
    extDict.SetAt("Metadata", xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    ed.WriteMessage($"\nAttached metadata to {ent.GetType().Name}");
    ed.WriteMessage($"\nExtension dictionary entries: {extDict.Count}");
    
    tr.Commit();
}
```

### Example 8: Removing Dictionary Entries
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyCompanyData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyCompanyData"), OpenMode.ForWrite) as DBDictionary;
        
        // Remove a specific entry
        string entryToRemove = "Settings";
        if (customDict.Contains(entryToRemove))
        {
            customDict.Remove(entryToRemove);
            ed.WriteMessage($"\nRemoved entry: {entryToRemove}");
        }
        
        // Remove all entries
        List<string> keysToRemove = new List<string>();
        foreach (DBDictionaryEntry entry in customDict)
        {
            keysToRemove.Add(entry.Key);
        }
        
        foreach (string key in keysToRemove)
        {
            customDict.Remove(key);
            ed.WriteMessage($"\nRemoved: {key}");
        }
        
        ed.WriteMessage($"\nRemaining entries: {customDict.Count}");
    }
    
    tr.Commit();
}
```

### Example 9: Hard vs Soft Ownership
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Create dictionary with hard ownership (default)
    DBDictionary hardDict = new DBDictionary();
    hardDict.TreatElementsAsHard = true;  // Entries are hard-owned (deleted with dictionary)
    nod.SetAt("HardOwnedDict", hardDict);
    tr.AddNewlyCreatedDBObject(hardDict, true);
    
    // Create dictionary with soft ownership
    DBDictionary softDict = new DBDictionary();
    softDict.TreatElementsAsHard = false;  // Entries are soft-owned (not deleted with dictionary)
    nod.SetAt("SoftOwnedDict", softDict);
    tr.AddNewlyCreatedDBObject(softDict, true);
    
    ed.WriteMessage("\nCreated dictionaries with different ownership:");
    ed.WriteMessage($"\n  HardOwnedDict: TreatElementsAsHard = {hardDict.TreatElementsAsHard}");
    ed.WriteMessage($"\n  SoftOwnedDict: TreatElementsAsHard = {softDict.TreatElementsAsHard}");
    
    tr.Commit();
}
```

### Example 10: Searching Nested Dictionaries Recursively
```csharp
public void SearchDictionariesRecursive(ObjectId dictId, Transaction tr, Editor ed, int level = 0)
{
    DBDictionary dict = tr.GetObject(dictId, OpenMode.ForRead) as DBDictionary;
    string indent = new string(' ', level * 2);
    
    foreach (DBDictionaryEntry entry in dict)
    {
        DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
        
        ed.WriteMessage($"\n{indent}{entry.Key} ({obj.GetType().Name})");
        
        if (obj is DBDictionary)
        {
            // Recursively search nested dictionary
            SearchDictionariesRecursive(entry.Value, tr, ed, level + 1);
        }
        else if (obj is XRecord)
        {
            XRecord xRec = obj as XRecord;
            ResultBuffer rb = xRec.Data;
            if (rb != null)
            {
                ed.WriteMessage($" - {rb.AsArray().Length} items");
                rb.Dispose();
            }
        }
    }
}

// Usage:
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ed.WriteMessage("\n=== Dictionary Hierarchy ===");
    SearchDictionariesRecursive(db.NamedObjectsDictionaryId, tr, ed);
    tr.Commit();
}
```

## Standard Database Dictionaries

The Database class provides access to several standard dictionaries:

| Property | Dictionary Purpose |
|----------|-------------------|
| `NamedObjectsDictionaryId` | Root dictionary for all named objects |
| `GroupDictionaryId` | Object groups |
| `LayoutDictionaryId` | Paper space layouts |
| `MLStyleDictionaryId` | Multiline styles |
| `PlotSettingsDictionaryId` | Plot configurations |
| `ColorDictionaryId` | Color definitions |
| `MaterialDictionaryId` | Material definitions |
| `VisualStyleDictionaryId` | Visual styles |

## Dictionary Ownership

### Hard Ownership (`TreatElementsAsHard = true`)
- Dictionary **owns** its entries
- When dictionary is erased, all entries are erased
- Entries cannot exist without the dictionary
- **Default for most dictionaries**

### Soft Ownership (`TreatElementsAsHard = false`)
- Dictionary **references** its entries
- When dictionary is erased, entries remain
- Entries can exist independently
- Used when multiple dictionaries reference the same objects

## Best Practices

1. **Check for Existence**: Always use `Contains()` before accessing dictionary entries
2. **Transaction Management**: Always use transactions when modifying dictionaries
3. **Naming Conventions**: Use descriptive, unique names for dictionary entries
4. **Hierarchical Organization**: Use nested dictionaries to organize complex data structures
5. **Extension Dictionaries**: Use entity extension dictionaries for entity-specific data
6. **Ownership Awareness**: Understand hard vs soft ownership implications
7. **Enumeration Safety**: Collect keys before removing entries during enumeration
8. **Dispose Resources**: Dispose ResultBuffers when reading XRecord data

## Common Use Cases

### Application Settings
Store application-wide settings in a custom dictionary under the NOD:
```
Named Objects Dictionary
  └─ MyAppSettings (DBDictionary)
      ├─ Configuration (XRecord)
      ├─ Preferences (XRecord)
      └─ LicenseInfo (XRecord)
```

### Project Organization
Organize project data hierarchically:
```
Named Objects Dictionary
  └─ ProjectData (DBDictionary)
      ├─ Structural (DBDictionary)
      │   ├─ Beams (XRecord)
      │   └─ Columns (XRecord)
      ├─ Mechanical (DBDictionary)
      └─ Electrical (DBDictionary)
```

### Entity Metadata
Attach custom data to specific entities:
```
Entity (Line, Circle, etc.)
  └─ Extension Dictionary
      ├─ Metadata (XRecord)
      ├─ CustomProperties (XRecord)
      └─ History (XRecord)
```

## Related Objects
- [XRecord](XRecord.md) - Custom data storage object used in dictionaries
- [Database](Database.md) - Provides access to standard dictionaries
- [Entity](../BaseClasses/Entity.md) - Can have extension dictionaries
- [Group](Group.md) - Stored in Group Dictionary
- [Layout](Layout.md) - Stored in Layout Dictionary
- DBDictionaryEntry - Key-value pair in dictionary enumeration

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [DBDictionary Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBDictionary)
- [Working with Dictionaries Guide](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-A809CD71-4655-44E2-B674-1FE200B9FE75)
