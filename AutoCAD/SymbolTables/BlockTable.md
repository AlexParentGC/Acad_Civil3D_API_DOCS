# BlockTable Class

## Overview
The `BlockTable` class is a symbol table that contains all block definitions in an AutoCAD drawing, including Model Space and Paper Space.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ BlockTable
```

## Key Properties

Inherits from `SymbolTable` - acts as a collection of `BlockTableRecord` objects.

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a block exists by name |
| `this[string]` | `ObjectId` | Gets block ObjectId by name (indexer) |

## Code Examples

### Example 1: Accessing Block Table
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Access Model Space
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Access Paper Space
    BlockTableRecord paperSpace = tr.GetObject(bt[BlockTableRecord.PaperSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    tr.Commit();
}
```

### Example 2: Listing All Blocks
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    ed.WriteMessage("\nBlocks in drawing:");
    
    foreach (ObjectId btrId in bt)
    {
        BlockTableRecord btr = tr.GetObject(btrId, OpenMode.ForRead) as BlockTableRecord;
        
        // Skip layout blocks and anonymous blocks
        if (!btr.IsLayout && !btr.IsAnonymous)
        {
            ed.WriteMessage($"\n  {btr.Name}");
        }
    }
    
    tr.Commit();
}
```

### Example 3: Checking if Block Exists
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    string blockName = "MyBlock";
    
    if (bt.Has(blockName))
    {
        ObjectId blockId = bt[blockName];
        BlockTableRecord btr = tr.GetObject(blockId, OpenMode.ForRead) as BlockTableRecord;
        
        ed.WriteMessage($"\nBlock '{blockName}' found");
    }
    else
    {
        ed.WriteMessage($"\nBlock '{blockName}' not found");
    }
    
    tr.Commit();
}
```

### Example 4: Creating a New Block Definition
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForWrite) as BlockTable;
    
    // Create new block table record
    BlockTableRecord btr = new BlockTableRecord();
    btr.Name = "MyNewBlock";
    
    // Add a circle to the block
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 10);
    btr.AppendEntity(circle);
    
    // Add block to table
    bt.Add(btr);
    tr.AddNewlyCreatedDBObject(btr, true);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains BlockTableId
- [BlockReference](../Entities/Complex/BlockReference.md) - Block insertion
- BlockTableRecord - Block definition (container)

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_BlockTable)
