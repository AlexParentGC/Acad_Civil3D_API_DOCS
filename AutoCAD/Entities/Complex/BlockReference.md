# BlockReference Class

## Overview
The `BlockReference` class represents an instance (insertion) of a block definition in AutoCAD. It's used to place copies of block definitions with specific position, scale, and rotation.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ BlockReference
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `BlockTableRecord` | `ObjectId` | Gets/sets the block definition ObjectId |
| `Position` | `Point3d` | Gets/sets the insertion point |
| `Rotation` | `double` | Gets/sets the rotation angle (radians) |
| `ScaleFactors` | `Scale3d` | Gets/sets the X, Y, Z scale factors |
| `Normal` | `Vector3d` | Gets/sets the normal vector |
| `AttributeCollection` | `AttributeCollection` | Gets the collection of attributes |
| `IsDynamicBlock` | `bool` | Gets whether this is a dynamic block |
| `DynamicBlockTableRecord` | `ObjectId` | Gets the dynamic block definition |
| `BlockTransform` | `Matrix3d` | Gets the transformation matrix |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetBlockReferenceIds(bool, bool)` | `ObjectIdCollection` | Gets all references to this block |
| `ExplodeToOwnerSpace()` | `void` | Explodes the block reference |
| `ResetBlock()` | `void` | Resets dynamic block to default state |

## Code Examples

### Example 1: Inserting a Block
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Check if block exists
    if (bt.Has("MyBlock"))
    {
        BlockReference blockRef = new BlockReference(new Point3d(100, 100, 0), bt["MyBlock"]);
        
        // Set properties
        blockRef.Rotation = Math.PI / 4; // 45 degrees
        blockRef.ScaleFactors = new Scale3d(2.0, 2.0, 1.0); // 2x scale in X and Y
        
        btr.AppendEntity(blockRef);
        tr.AddNewlyCreatedDBObject(blockRef, true);
    }
    
    tr.Commit();
}
```

### Example 2: Working with Block Attributes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    if (bt.Has("TitleBlock"))
    {
        BlockReference blockRef = new BlockReference(new Point3d(0, 0, 0), bt["TitleBlock"]);
        btr.AppendEntity(blockRef);
        tr.AddNewlyCreatedDBObject(blockRef, true);
        
        // Get block definition to find attribute definitions
        BlockTableRecord blockDef = tr.GetObject(bt["TitleBlock"], OpenMode.ForRead) as BlockTableRecord;
        
        // Add attributes
        foreach (ObjectId objId in blockDef)
        {
            DBObject obj = tr.GetObject(objId, OpenMode.ForRead);
            
            if (obj is AttributeDefinition attDef)
            {
                AttributeReference attRef = new AttributeReference();
                attRef.SetAttributeFromBlock(attDef, blockRef.BlockTransform);
                attRef.TextString = "Value for " + attDef.Tag;
                
                blockRef.AttributeCollection.AppendAttribute(attRef);
                tr.AddNewlyCreatedDBObject(attRef, true);
            }
        }
    }
    
    tr.Commit();
}
```

### Example 3: Reading Block Attributes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockReference blockRef = tr.GetObject(blockRefId, OpenMode.ForRead) as BlockReference;
    
    foreach (ObjectId attId in blockRef.AttributeCollection)
    {
        AttributeReference attRef = tr.GetObject(attId, OpenMode.ForRead) as AttributeReference;
        
        ed.WriteMessage($"\n{attRef.Tag}: {attRef.TextString}");
    }
    
    tr.Commit();
}
```

### Example 4: Finding All Instances of a Block
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    if (bt.Has("MyBlock"))
    {
        BlockTableRecord blockDef = tr.GetObject(bt["MyBlock"], OpenMode.ForRead) as BlockTableRecord;
        
        // Get all references to this block
        ObjectIdCollection refIds = blockDef.GetBlockReferenceIds(true, true);
        
        ed.WriteMessage($"\nFound {refIds.Count} instances of 'MyBlock'");
        
        foreach (ObjectId refId in refIds)
        {
            BlockReference blockRef = tr.GetObject(refId, OpenMode.ForRead) as BlockReference;
            ed.WriteMessage($"\n  Position: {blockRef.Position}");
        }
    }
    
    tr.Commit();
}
```

## Related Objects
- [BlockTable](../../SymbolTables/BlockTable.md) - Collection of block definitions
- [BlockTableRecord](../../Core/BlockTableRecord.md) - Block definition
- [AttributeReference](AttributeReference.md) - Block attribute instance
- [Entity](../../BaseClasses/Entity.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_BlockReference)
