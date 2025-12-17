# AttributeReference

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `AttributeReference` class represents an instance of an attribute within a block reference. It contains the actual value of the attribute as defined by its corresponding AttributeDefinition.

**Key Concept:** AttributeReferences are created automatically when a block containing AttributeDefinitions is inserted. They hold the actual data values.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ DBText
                  └─ AttributeReference
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Tag` | `string` | Gets the attribute tag (from definition) |
| `TextString` | `string` | Gets/sets the attribute value |
| `Invisible` | `bool` | Gets/sets whether attribute is visible |
| `IsMTextAttribute` | `bool` | Checks if attribute is multiline text |
| `FieldLength` | `int` | Gets/sets maximum field length |

## Common Usage Patterns

### 1. Reading Attributes from Block Reference

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("READATTS")]
public void ReadAttributesFromBlock()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select block reference
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect block: ");
    peo.SetRejectMessage("\nMust be a block reference.");
    peo.AddAllowedClass(typeof(BlockReference), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockReference blockRef = tr.GetObject(per.ObjectId, 
            OpenMode.ForRead) as BlockReference;
        
        ed.WriteMessage($"\n=== Attributes in Block '{blockRef.Name}' ===");
        
        AttributeCollection attCol = blockRef.AttributeCollection;
        
        foreach (ObjectId attId in attCol)
        {
            AttributeReference attRef = tr.GetObject(attId, 
                OpenMode.ForRead) as AttributeReference;
            
            ed.WriteMessage($"\n{attRef.Tag}: {attRef.TextString}");
        }
        
        tr.Commit();
    }
}
```

### 2. Modifying Attribute Values

```csharp
[CommandMethod("MODIFYATTS")]
public void ModifyAttributeValues()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select block
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect block: ");
    peo.SetRejectMessage("\nMust be a block reference.");
    peo.AddAllowedClass(typeof(BlockReference), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockReference blockRef = tr.GetObject(per.ObjectId, 
            OpenMode.ForRead) as BlockReference;
        
        AttributeCollection attCol = blockRef.AttributeCollection;
        
        foreach (ObjectId attId in attCol)
        {
            AttributeReference attRef = tr.GetObject(attId, 
                OpenMode.ForWrite) as AttributeReference;
            
            if (attRef.Tag == "DATE")
            {
                attRef.TextString = DateTime.Now.ToShortDateString();
                ed.WriteMessage($"\nUpdated {attRef.Tag} to {attRef.TextString}");
            }
            else if (attRef.Tag == "REVISION")
            {
                int rev = int.Parse(attRef.TextString);
                attRef.TextString = (rev + 1).ToString();
                ed.WriteMessage($"\nIncremented {attRef.Tag} to {attRef.TextString}");
            }
        }
        
        tr.Commit();
    }
}
```

### 3. Finding Specific Attribute by Tag

```csharp
[CommandMethod("FINDATT")]
public void FindAttributeByTag()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for tag name
    PromptStringOptions pso = new PromptStringOptions("\nEnter attribute tag to find: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string searchTag = pr.StringResult.ToUpper();
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForRead) as BlockTableRecord;
        
        int foundCount = 0;
        
        foreach (ObjectId id in btr)
        {
            if (id.ObjectClass.Name == "AcDbBlockReference")
            {
                BlockReference blockRef = tr.GetObject(id, OpenMode.ForRead) as BlockReference;
                AttributeCollection attCol = blockRef.AttributeCollection;
                
                foreach (ObjectId attId in attCol)
                {
                    AttributeReference attRef = tr.GetObject(attId, 
                        OpenMode.ForRead) as AttributeReference;
                    
                    if (attRef.Tag == searchTag)
                    {
                        foundCount++;
                        ed.WriteMessage($"\nFound in block '{blockRef.Name}': {attRef.TextString}");
                    }
                }
            }
        }
        
        ed.WriteMessage($"\n\nTotal found: {foundCount}");
        tr.Commit();
    }
}
```

### 4. Extracting Attributes to CSV

```csharp
[CommandMethod("EXTRACTATTS")]
public void ExtractAttributesToCSV()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForRead) as BlockTableRecord;
        
        System.Text.StringBuilder csv = new System.Text.StringBuilder();
        csv.AppendLine("Block Name,Tag,Value,Position");
        
        foreach (ObjectId id in btr)
        {
            if (id.ObjectClass.Name == "AcDbBlockReference")
            {
                BlockReference blockRef = tr.GetObject(id, OpenMode.ForRead) as BlockReference;
                AttributeCollection attCol = blockRef.AttributeCollection;
                
                foreach (ObjectId attId in attCol)
                {
                    AttributeReference attRef = tr.GetObject(attId, 
                        OpenMode.ForRead) as AttributeReference;
                    
                    csv.AppendLine($"{blockRef.Name},{attRef.Tag}," +
                        $"\"{attRef.TextString}\",\"{attRef.Position}\"");
                }
            }
        }
        
        // Save to file
        string filePath = @"C:\Temp\attributes.csv";
        System.IO.File.WriteAllText(filePath, csv.ToString());
        
        ed.WriteMessage($"\nAttributes exported to {filePath}");
        tr.Commit();
    }
}
```

### 5. Synchronizing Attributes with Block Definition

```csharp
[CommandMethod("SYNCATTS")]
public void SynchronizeAttributes()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select block
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect block to sync: ");
    peo.SetRejectMessage("\nMust be a block reference.");
    peo.AddAllowedClass(typeof(BlockReference), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockReference blockRef = tr.GetObject(per.ObjectId, 
            OpenMode.ForWrite) as BlockReference;
        
        // Synchronize attributes with block definition
        blockRef.ResetAttributes();
        
        ed.WriteMessage("\nAttributes synchronized with block definition");
        tr.Commit();
    }
}
```

### 6. Batch Update Attributes

```csharp
[CommandMethod("BATCHUPDATEATTS")]
public void BatchUpdateAttributes()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for block name
    PromptStringOptions pso = new PromptStringOptions("\nEnter block name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string blockName = pr.StringResult;
    
    // Prompt for tag
    pso = new PromptStringOptions("\nEnter attribute tag: ");
    pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string tag = pr.StringResult.ToUpper();
    
    // Prompt for new value
    pso = new PromptStringOptions("\nEnter new value: ");
    pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string newValue = pr.StringResult;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForRead) as BlockTableRecord;
        
        int updateCount = 0;
        
        foreach (ObjectId id in btr)
        {
            if (id.ObjectClass.Name == "AcDbBlockReference")
            {
                BlockReference blockRef = tr.GetObject(id, OpenMode.ForRead) as BlockReference;
                
                if (blockRef.Name == blockName)
                {
                    AttributeCollection attCol = blockRef.AttributeCollection;
                    
                    foreach (ObjectId attId in attCol)
                    {
                        AttributeReference attRef = tr.GetObject(attId, 
                            OpenMode.ForWrite) as AttributeReference;
                        
                        if (attRef.Tag == tag)
                        {
                            attRef.TextString = newValue;
                            updateCount++;
                        }
                    }
                }
            }
        }
        
        tr.Commit();
        ed.WriteMessage($"\nUpdated {updateCount} attributes");
    }
}
```

## Best Practices

1. **Read-Only Access**: Open for read when only querying values
2. **Batch Updates**: Group multiple updates in single transaction
3. **Tag Matching**: Use case-insensitive tag comparison
4. **Null Checks**: Verify AttributeCollection is not empty
5. **Sync After Definition Changes**: Use ResetAttributes() after modifying block definition
6. **Data Validation**: Validate attribute values before setting
7. **Invisible Attributes**: Remember to check invisible attributes for data extraction

## Common Patterns

### Pattern 1: Read All Attributes
```csharp
foreach (ObjectId attId in blockRef.AttributeCollection)
{
    AttributeReference attRef = tr.GetObject(attId, OpenMode.ForRead) as AttributeReference;
    // Process attribute
}
```

### Pattern 2: Find Specific Attribute
```csharp
foreach (ObjectId attId in blockRef.AttributeCollection)
{
    AttributeReference attRef = tr.GetObject(attId, OpenMode.ForRead) as AttributeReference;
    if (attRef.Tag == "TARGET_TAG")
    {
        // Found it
    }
}
```

### Pattern 3: Update Attribute Value
```csharp
AttributeReference attRef = tr.GetObject(attId, OpenMode.ForWrite) as AttributeReference;
attRef.TextString = "New Value";
```

## Related Classes

- [AttributeDefinition](AttributeDefinition.md) - Template for attributes in block definition
- [BlockReference](../Entities/Complex/BlockReference.md) - Block insertion containing attributes
- [AttributeCollection](AttributeCollection.md) - Collection of attributes in block reference
- [DBText](../Entities/Text/DBText.md) - Base class for attribute references

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-AttributeReference)
