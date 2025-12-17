# AttributeDefinition

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `AttributeDefinition` class defines a template for attributes within a block definition. It specifies the tag, prompt, default value, and properties for attributes that will be created when the block is inserted.

**Key Concept:** AttributeDefinitions exist in block definitions and serve as templates. When a block is inserted, AttributeReferences are created based on these definitions.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ DBText
                  └─ AttributeDefinition
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Tag` | `string` | Gets/sets the attribute tag (identifier) |
| `Prompt` | `string` | Gets/sets the prompt shown when inserting block |
| `TextString` | `string` | Gets/sets the default value |
| `Invisible` | `bool` | Gets/sets whether attribute is invisible |
| `Constant` | `bool` | Gets/sets whether attribute value is constant |
| `Verifiable` | `bool` | Gets/sets whether to verify value on insertion |
| `Preset` | `bool` | Gets/sets whether attribute is preset |
| `LockPositionInBlock` | `bool` | Gets/sets whether position is locked in block |
| `IsMTextAttributeDefinition` | `bool` | Checks if attribute supports multiline text |

## Common Usage Patterns

### 1. Creating Attribute Definition

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Geometry;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("CREATEATTDEF")]
public void CreateAttributeDefinition()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForWrite) as BlockTableRecord;
        
        // Create attribute definition
        AttributeDefinition attDef = new AttributeDefinition();
        attDef.SetDatabaseDefaults();
        attDef.Position = new Point3d(0, 0, 0);
        attDef.Tag = "PART_NUMBER";
        attDef.Prompt = "Enter part number: ";
        attDef.TextString = "12345";
        attDef.Height = 2.5;
        attDef.Justify = AttachmentPoint.MiddleCenter;
        
        btr.AppendEntity(attDef);
        tr.AddNewlyCreatedDBObject(attDef, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nAttribute definition created");
    }
}
```

### 2. Creating Block with Attributes

```csharp
[CommandMethod("CREATEATTBLOCK")]
public void CreateBlockWithAttributes()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForWrite) as BlockTable;
        
        // Create new block definition
        BlockTableRecord btr = new BlockTableRecord();
        btr.Name = "TITLE_BLOCK";
        btr.Origin = Point3d.Origin;
        
        ObjectId btrId = bt.Add(btr);
        tr.AddNewlyCreatedDBObject(btr, true);
        
        // Add rectangle to block
        Polyline rect = new Polyline();
        rect.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0);
        rect.AddVertexAt(1, new Point2d(100, 0), 0, 0, 0);
        rect.AddVertexAt(2, new Point2d(100, 50), 0, 0, 0);
        rect.AddVertexAt(3, new Point2d(0, 50), 0, 0, 0);
        rect.Closed = true;
        
        btr.AppendEntity(rect);
        tr.AddNewlyCreatedDBObject(rect, true);
        
        // Add attribute definitions
        AttributeDefinition attDef1 = new AttributeDefinition();
        attDef1.SetDatabaseDefaults();
        attDef1.Position = new Point3d(10, 40, 0);
        attDef1.Tag = "TITLE";
        attDef1.Prompt = "Enter drawing title: ";
        attDef1.TextString = "DRAWING TITLE";
        attDef1.Height = 5.0;
        
        btr.AppendEntity(attDef1);
        tr.AddNewlyCreatedDBObject(attDef1, true);
        
        AttributeDefinition attDef2 = new AttributeDefinition();
        attDef2.SetDatabaseDefaults();
        attDef2.Position = new Point3d(10, 30, 0);
        attDef2.Tag = "DATE";
        attDef2.Prompt = "Enter date: ";
        attDef2.TextString = DateTime.Now.ToShortDateString();
        attDef2.Height = 3.0;
        
        btr.AppendEntity(attDef2);
        tr.AddNewlyCreatedDBObject(attDef2, true);
        
        AttributeDefinition attDef3 = new AttributeDefinition();
        attDef3.SetDatabaseDefaults();
        attDef3.Position = new Point3d(10, 20, 0);
        attDef3.Tag = "DRAWN_BY";
        attDef3.Prompt = "Enter drafter name: ";
        attDef3.TextString = "INITIALS";
        attDef3.Height = 3.0;
        
        btr.AppendEntity(attDef3);
        tr.AddNewlyCreatedDBObject(attDef3, true);
        
        tr.Commit();
        ed.WriteMessage($"\nBlock '{btr.Name}' created with 3 attributes");
    }
}
```

### 3. Reading Attribute Definitions from Block

```csharp
[CommandMethod("LISTATTDEFS")]
public void ListAttributeDefinitions()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for block name
    PromptStringOptions pso = new PromptStringOptions("\nEnter block name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        
        if (!bt.Has(pr.StringResult))
        {
            ed.WriteMessage($"\nBlock '{pr.StringResult}' not found");
            tr.Commit();
            return;
        }
        
        BlockTableRecord btr = tr.GetObject(bt[pr.StringResult], 
            OpenMode.ForRead) as BlockTableRecord;
        
        ed.WriteMessage($"\n=== Attributes in Block '{btr.Name}' ===");
        
        int count = 0;
        foreach (ObjectId id in btr)
        {
            if (id.ObjectClass.Name == "AcDbAttributeDefinition")
            {
                AttributeDefinition attDef = tr.GetObject(id, 
                    OpenMode.ForRead) as AttributeDefinition;
                
                count++;
                ed.WriteMessage($"\n\nAttribute {count}:");
                ed.WriteMessage($"\n  Tag: {attDef.Tag}");
                ed.WriteMessage($"\n  Prompt: {attDef.Prompt}");
                ed.WriteMessage($"\n  Default: {attDef.TextString}");
                ed.WriteMessage($"\n  Height: {attDef.Height}");
                ed.WriteMessage($"\n  Invisible: {attDef.Invisible}");
                ed.WriteMessage($"\n  Constant: {attDef.Constant}");
            }
        }
        
        if (count == 0)
            ed.WriteMessage("\nNo attributes found in this block");
        
        tr.Commit();
    }
}
```

### 4. Modifying Attribute Definitions

```csharp
[CommandMethod("MODIFYATTDEF")]
public void ModifyAttributeDefinition()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select attribute definition
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect attribute definition: ");
    peo.SetRejectMessage("\nMust be an attribute definition.");
    peo.AddAllowedClass(typeof(AttributeDefinition), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        AttributeDefinition attDef = tr.GetObject(per.ObjectId, 
            OpenMode.ForWrite) as AttributeDefinition;
        
        // Modify properties
        attDef.Prompt = "Updated prompt: ";
        attDef.TextString = "NEW_DEFAULT";
        attDef.Height = 5.0;
        attDef.ColorIndex = 1; // Red
        
        ed.WriteMessage($"\nModified attribute '{attDef.Tag}'");
        
        tr.Commit();
    }
}
```

### 5. Creating Invisible and Constant Attributes

```csharp
[CommandMethod("CREATESPECIALATT")]
public void CreateSpecialAttributes()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForWrite) as BlockTableRecord;
        
        // Invisible attribute (hidden but extractable)
        AttributeDefinition invisibleAtt = new AttributeDefinition();
        invisibleAtt.SetDatabaseDefaults();
        invisibleAtt.Position = new Point3d(0, 0, 0);
        invisibleAtt.Tag = "PART_ID";
        invisibleAtt.TextString = "ABC-123";
        invisibleAtt.Height = 2.5;
        invisibleAtt.Invisible = true; // Won't display but can be extracted
        
        btr.AppendEntity(invisibleAtt);
        tr.AddNewlyCreatedDBObject(invisibleAtt, true);
        
        // Constant attribute (value cannot change)
        AttributeDefinition constantAtt = new AttributeDefinition();
        constantAtt.SetDatabaseDefaults();
        constantAtt.Position = new Point3d(0, 10, 0);
        constantAtt.Tag = "COMPANY";
        constantAtt.TextString = "ACME Corporation";
        constantAtt.Height = 2.5;
        constantAtt.Constant = true; // Value is fixed
        
        btr.AppendEntity(constantAtt);
        tr.AddNewlyCreatedDBObject(constantAtt, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nCreated invisible and constant attributes");
    }
}
```

### 6. Multiline Text Attribute Definition

```csharp
[CommandMethod("CREATEMTEXTATT")]
public void CreateMTextAttributeDefinition()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForWrite) as BlockTableRecord;
        
        // Create MText attribute definition
        AttributeDefinition attDef = new AttributeDefinition();
        attDef.SetDatabaseDefaults();
        attDef.Position = new Point3d(0, 0, 0);
        attDef.Tag = "NOTES";
        attDef.Prompt = "Enter notes: ";
        attDef.TextString = "Enter detailed notes here...";
        attDef.Height = 2.5;
        
        // Convert to MText attribute
        attDef.ConvertToMTextAttribute();
        
        btr.AppendEntity(attDef);
        tr.AddNewlyCreatedDBObject(attDef, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nMText attribute definition created");
    }
}
```

## Best Practices

1. **Unique Tags**: Use unique, descriptive tags (e.g., "PART_NUMBER" not "ATT1")
2. **Clear Prompts**: Provide helpful prompts for users
3. **Default Values**: Set reasonable defaults
4. **Height**: Use consistent text heights
5. **Invisible vs Constant**: Use invisible for hidden data, constant for fixed values
6. **Position**: Place attributes logically within block geometry
7. **Verification**: Use Verifiable flag for critical data

## Common Patterns

### Pattern 1: Standard Attribute
```csharp
AttributeDefinition attDef = new AttributeDefinition();
attDef.SetDatabaseDefaults();
attDef.Tag = "TAG_NAME";
attDef.Prompt = "Enter value: ";
attDef.TextString = "DEFAULT";
attDef.Height = 2.5;
```

### Pattern 2: Invisible Data Attribute
```csharp
attDef.Invisible = true;
attDef.Tag = "DATA_ID";
attDef.TextString = "12345";
```

### Pattern 3: Fixed Value Attribute
```csharp
attDef.Constant = true;
attDef.Tag = "COMPANY";
attDef.TextString = "Company Name";
```

## Related Classes

- [AttributeReference](AttributeReference.md) - Instance of attribute in block reference
- [BlockReference](../Entities/Complex/BlockReference.md) - Block insertion containing attributes
- [BlockTableRecord](../SymbolTables/BlockTable.md) - Block definition containing attribute definitions
- [DBText](../Entities/Text/DBText.md) - Base class for attribute definitions

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-AttributeDefinition)
