# XRecord Class

## Overview
The `XRecord` class is a custom data storage object that can be stored in dictionaries (such as the Named Objects Dictionary or extension dictionaries). Unlike XData which is limited to ~16KB and attached directly to entities, XRecords can store unlimited amounts of structured data and are stored in dictionaries as named entries.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ XRecord
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | `ResultBuffer` | Gets/sets the data stored in the XRecord |
| `IsClonable` | `bool` | Gets whether the XRecord can be cloned |
| `MergeStyle` | `DictionaryMergeStyle` | Gets/sets how the XRecord is merged during WBLOCK operations |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `XRecord()` | Constructor | Creates a new empty XRecord |
| `Dispose()` | `void` | Disposes the XRecord and releases resources |

## Code Examples

### Example 1: Creating an XRecord in Named Objects Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get the Named Objects Dictionary (NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Create a custom dictionary if it doesn't exist
    string dictName = "MyAppData";
    DBDictionary customDict;
    
    if (nod.Contains(dictName))
    {
        customDict = tr.GetObject(nod.GetAt(dictName), OpenMode.ForWrite) as DBDictionary;
    }
    else
    {
        customDict = new DBDictionary();
        nod.SetAt(dictName, customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
    }
    
    // Create an XRecord with data
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "Project Name"),
        new TypedValue((int)DxfCode.Int32, 12345),
        new TypedValue((int)DxfCode.Real, 3.14159)
    );
    xRec.Data = rb;
    
    // Add XRecord to custom dictionary
    customDict.SetAt("ProjectInfo", xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    tr.Commit();
}
```

### Example 2: Reading XRecord Data
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyAppData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyAppData"), OpenMode.ForRead) as DBDictionary;
        
        if (customDict.Contains("ProjectInfo"))
        {
            XRecord xRec = tr.GetObject(customDict.GetAt("ProjectInfo"), OpenMode.ForRead) as XRecord;
            
            ResultBuffer rb = xRec.Data;
            
            if (rb != null)
            {
                foreach (TypedValue tv in rb)
                {
                    ed.WriteMessage($"\nCode: {tv.TypeCode}, Value: {tv.Value}");
                }
                
                rb.Dispose();
            }
        }
    }
    
    tr.Commit();
}
```

### Example 3: Storing Complex Structured Data
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Create XRecord with structured data
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        // Header
        new TypedValue((int)DxfCode.Text, "HEADER"),
        new TypedValue((int)DxfCode.Text, "Version"),
        new TypedValue((int)DxfCode.Text, "1.0"),
        
        // Project info
        new TypedValue((int)DxfCode.Text, "PROJECT"),
        new TypedValue((int)DxfCode.Text, "Name"),
        new TypedValue((int)DxfCode.Text, "Building A"),
        new TypedValue((int)DxfCode.Text, "Date"),
        new TypedValue((int)DxfCode.Text, DateTime.Now.ToString("yyyy-MM-dd")),
        
        // Numeric data
        new TypedValue((int)DxfCode.Text, "METRICS"),
        new TypedValue((int)DxfCode.Real, 1234.56),
        new TypedValue((int)DxfCode.Real, 7890.12),
        
        // 3D Point
        new TypedValue((int)DxfCode.Point3d, new Point3d(100, 200, 0))
    );
    xRec.Data = rb;
    
    if (!nod.Contains("AppSettings"))
    {
        DBDictionary appDict = new DBDictionary();
        nod.SetAt("AppSettings", appDict);
        tr.AddNewlyCreatedDBObject(appDict, true);
        
        appDict.SetAt("Config", xRec);
        tr.AddNewlyCreatedDBObject(xRec, true);
    }
    
    rb.Dispose();
    
    tr.Commit();
}
```

### Example 4: Attaching XRecord to Entity Extension Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get an entity (e.g., a line)
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Create or get the entity's extension dictionary
    if (ent.ExtensionDictionary == ObjectId.Null)
    {
        ent.CreateExtensionDictionary();
    }
    
    DBDictionary extDict = tr.GetObject(ent.ExtensionDictionary, OpenMode.ForWrite) as DBDictionary;
    
    // Create XRecord with custom data
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "CustomProperty"),
        new TypedValue((int)DxfCode.Text, "CustomValue"),
        new TypedValue((int)DxfCode.Int32, 42)
    );
    xRec.Data = rb;
    
    // Add to extension dictionary
    string key = "MyAppData";
    if (extDict.Contains(key))
    {
        extDict.Remove(key);
    }
    
    extDict.SetAt(key, xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    ed.WriteMessage($"\nAttached XRecord to entity {ent.GetType().Name}");
    
    tr.Commit();
}
```

### Example 5: Updating XRecord Data
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyAppData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyAppData"), OpenMode.ForRead) as DBDictionary;
        
        if (customDict.Contains("ProjectInfo"))
        {
            XRecord xRec = tr.GetObject(customDict.GetAt("ProjectInfo"), OpenMode.ForWrite) as XRecord;
            
            // Update the data
            ResultBuffer newRb = new ResultBuffer(
                new TypedValue((int)DxfCode.Text, "Updated Project Name"),
                new TypedValue((int)DxfCode.Int32, 99999),
                new TypedValue((int)DxfCode.Real, 2.71828),
                new TypedValue((int)DxfCode.Text, "New Field")
            );
            
            xRec.Data = newRb;
            newRb.Dispose();
            
            ed.WriteMessage("\nXRecord data updated successfully");
        }
    }
    
    tr.Commit();
}
```

### Example 6: Deleting an XRecord
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyAppData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyAppData"), OpenMode.ForWrite) as DBDictionary;
        
        if (customDict.Contains("ProjectInfo"))
        {
            // Remove the XRecord from the dictionary
            customDict.Remove("ProjectInfo");
            
            ed.WriteMessage("\nXRecord removed successfully");
        }
    }
    
    tr.Commit();
}
```

### Example 7: Searching for XRecords in Named Objects Dictionary
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\n=== Searching for XRecords in NOD ===");
    
    foreach (DBDictionaryEntry entry in nod)
    {
        DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
        
        if (obj is XRecord)
        {
            XRecord xRec = obj as XRecord;
            ed.WriteMessage($"\nFound XRecord: {entry.Key}");
            
            ResultBuffer rb = xRec.Data;
            if (rb != null)
            {
                ed.WriteMessage($"  Data items: {rb.AsArray().Length}");
                rb.Dispose();
            }
        }
        else if (obj is DBDictionary)
        {
            // Recursively search nested dictionaries
            DBDictionary subDict = obj as DBDictionary;
            ed.WriteMessage($"\nSearching dictionary: {entry.Key}");
            
            foreach (DBDictionaryEntry subEntry in subDict)
            {
                DBObject subObj = tr.GetObject(subEntry.Value, OpenMode.ForRead);
                if (subObj is XRecord)
                {
                    ed.WriteMessage($"  Found XRecord: {subEntry.Key}");
                }
            }
        }
    }
    
    tr.Commit();
}
```

### Example 8: Storing Binary Data in XRecord
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Create XRecord with binary data
    XRecord xRec = new XRecord();
    
    // Example: Store a serialized object as binary data
    byte[] binaryData = new byte[] { 0x01, 0x02, 0x03, 0x04, 0xFF, 0xFE };
    
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "BinaryDataRecord"),
        new TypedValue((int)DxfCode.BinaryChunk, binaryData),
        new TypedValue((int)DxfCode.Int32, binaryData.Length)
    );
    xRec.Data = rb;
    
    if (!nod.Contains("BinaryData"))
    {
        DBDictionary binaryDict = new DBDictionary();
        nod.SetAt("BinaryData", binaryDict);
        tr.AddNewlyCreatedDBObject(binaryDict, true);
        
        binaryDict.SetAt("Data1", xRec);
        tr.AddNewlyCreatedDBObject(xRec, true);
    }
    
    rb.Dispose();
    
    ed.WriteMessage($"\nStored {binaryData.Length} bytes in XRecord");
    
    tr.Commit();
}
```

## Common DXF Codes for XRecord Data

| Code | Type | Description |
|------|------|-------------|
| `1` | String | Text string |
| `10` | Point3d | 3D point |
| `40` | Double | Floating-point value |
| `70` | Int16 | 16-bit integer |
| `90` | Int32 | 32-bit integer |
| `280` | Byte | 8-bit integer |
| `310` | Binary | Binary data chunk |
| `330` | ObjectId | Soft-pointer ID/handle |
| `340` | ObjectId | Hard-pointer ID/handle |
| `360` | ObjectId | Hard-owner ID/handle |

## Best Practices

1. **Use Dictionaries for Organization**: Store XRecords in custom dictionaries within the Named Objects Dictionary for better organization
2. **Dispose ResultBuffers**: Always dispose ResultBuffer objects to prevent memory leaks
3. **Use Meaningful Keys**: Use descriptive dictionary keys for XRecords
4. **Structure Your Data**: Plan your data structure using consistent DXF codes
5. **Extension Dictionaries**: Use entity extension dictionaries to attach XRecords to specific entities
6. **Transaction Management**: Always use transactions when creating or modifying XRecords
7. **Check for Existence**: Check if dictionary entries exist before accessing them
8. **Document Your Schema**: Document the structure and DXF codes used in your XRecords

## XRecord vs XData

| Feature | XRecord | XData |
|---------|---------|-------|
| Storage Location | Dictionaries (NOD, Extension Dict) | Attached directly to entities |
| Size Limit | Unlimited | ~16KB per entity |
| Structure | Stored as named entries | Identified by application name |
| Organization | Hierarchical (nested dictionaries) | Flat (per entity) |
| Accessibility | Requires dictionary navigation | Direct entity property |
| Use Case | Large/complex data, shared data | Small entity-specific data |
| Performance | Slightly slower (dictionary lookup) | Faster (direct access) |

## When to Use XRecord

- **Large Data**: When you need to store more than 16KB of data
- **Shared Data**: When multiple entities need to reference the same data
- **Complex Structures**: When you need hierarchical or nested data structures
- **Drawing-Level Data**: When storing application settings or drawing-wide configuration
- **Entity Metadata**: When attaching detailed metadata to specific entities via extension dictionaries

## Related Objects
- [DBDictionary](DBDictionary.md) - Container for XRecords
- [Database](Database.md) - Provides access to Named Objects Dictionary
- [XData](../BaseClasses/XData.md) - Alternative for small entity-specific data
- [Entity](../BaseClasses/Entity.md) - Can have extension dictionaries containing XRecords
- ResultBuffer - Data container for XRecord content
- TypedValue - Individual data values in ResultBuffer

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [XRecord Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Xrecord)
- [Working with Dictionaries](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-A809CD71-4655-44E2-B674-1FE200B9FE75)
