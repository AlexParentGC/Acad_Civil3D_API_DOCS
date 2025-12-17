# Group

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `Group` class represents a named collection of entities that can be selected and manipulated as a single unit. Groups provide a way to organize related objects without creating a block.

**Key Concept:** Groups are like selection sets that persist in the drawing. Unlike blocks, grouped entities maintain their individual properties and can be edited independently.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ Group
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the group name |
| `Description` | `string` | Gets/sets the group description |
| `Selectable` | `bool` | Gets/sets whether group can be selected as unit |
| `IsAnonymous` | `bool` | Checks if group is anonymous (unnamed) |
| `NumEntities` | `int` | Gets the number of entities in group |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Append(ObjectId)` | `void` | Adds entity to group |
| `Remove(ObjectId)` | `void` | Removes entity from group |
| `GetAllEntityIds()` | `ObjectId[]` | Gets all entity IDs in group |
| `Has(ObjectId)` | `bool` | Checks if entity is in group |
| `Clear()` | `void` | Removes all entities from group |

## Common Usage Patterns

### 1. Creating a Group

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("CREATEGROUP")]
public void CreateGroup()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for group name
    PromptStringOptions pso = new PromptStringOptions("\nEnter group name: ");
    pso.AllowSpaces = false;
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string groupName = pr.StringResult;
    
    // Select entities
    PromptSelectionResult psr = ed.GetSelection();
    if (psr.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
            OpenMode.ForWrite) as DBDictionary;
        
        // Create new group
        Group group = new Group("Group description", true);
        group.Name = groupName;
        
        // Add to group dictionary
        groupDict.SetAt(groupName, group);
        tr.AddNewlyCreatedDBObject(group, true);
        
        // Add entities to group
        foreach (SelectedObject so in psr.Value)
        {
            group.Append(so.ObjectId);
        }
        
        tr.Commit();
        ed.WriteMessage($"\nGroup '{groupName}' created with {group.NumEntities} entities");
    }
}
```

### 2. Listing All Groups

```csharp
[CommandMethod("LISTGROUPS")]
public void ListAllGroups()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        ed.WriteMessage("\n=== Groups ===");
        
        foreach (DBDictionaryEntry entry in groupDict)
        {
            Group group = tr.GetObject(entry.Value, OpenMode.ForRead) as Group;
            
            ed.WriteMessage($"\n\nName: {group.Name}");
            ed.WriteMessage($"\n  Description: {group.Description}");
            ed.WriteMessage($"\n  Entities: {group.NumEntities}");
            ed.WriteMessage($"\n  Selectable: {group.Selectable}");
            ed.WriteMessage($"\n  Anonymous: {group.IsAnonymous}");
        }
        
        tr.Commit();
    }
}
```

### 3. Adding Entities to Existing Group

```csharp
[CommandMethod("ADDTOGROUP")]
public void AddEntitiesToGroup()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for group name
    PromptStringOptions pso = new PromptStringOptions("\nEnter group name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string groupName = pr.StringResult;
    
    // Select entities to add
    PromptSelectionResult psr = ed.GetSelection();
    if (psr.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        if (!groupDict.Contains(groupName))
        {
            ed.WriteMessage($"\nGroup '{groupName}' not found");
            tr.Commit();
            return;
        }
        
        Group group = tr.GetObject(groupDict.GetAt(groupName), 
            OpenMode.ForWrite) as Group;
        
        int addedCount = 0;
        foreach (SelectedObject so in psr.Value)
        {
            if (!group.Has(so.ObjectId))
            {
                group.Append(so.ObjectId);
                addedCount++;
            }
        }
        
        tr.Commit();
        ed.WriteMessage($"\nAdded {addedCount} entities to group '{groupName}'");
    }
}
```

### 4. Removing Entities from Group

```csharp
[CommandMethod("REMOVEFROMGROUP")]
public void RemoveEntitiesFromGroup()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for group name
    PromptStringOptions pso = new PromptStringOptions("\nEnter group name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string groupName = pr.StringResult;
    
    // Select entities to remove
    PromptSelectionResult psr = ed.GetSelection();
    if (psr.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        if (!groupDict.Contains(groupName))
        {
            ed.WriteMessage($"\nGroup '{groupName}' not found");
            tr.Commit();
            return;
        }
        
        Group group = tr.GetObject(groupDict.GetAt(groupName), 
            OpenMode.ForWrite) as Group;
        
        int removedCount = 0;
        foreach (SelectedObject so in psr.Value)
        {
            if (group.Has(so.ObjectId))
            {
                group.Remove(so.ObjectId);
                removedCount++;
            }
        }
        
        tr.Commit();
        ed.WriteMessage($"\nRemoved {removedCount} entities from group '{groupName}'");
    }
}
```

### 5. Selecting All Entities in Group

```csharp
[CommandMethod("SELECTGROUP")]
public void SelectEntitiesInGroup()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for group name
    PromptStringOptions pso = new PromptStringOptions("\nEnter group name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string groupName = pr.StringResult;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        if (!groupDict.Contains(groupName))
        {
            ed.WriteMessage($"\nGroup '{groupName}' not found");
            tr.Commit();
            return;
        }
        
        Group group = tr.GetObject(groupDict.GetAt(groupName), 
            OpenMode.ForRead) as Group;
        
        ObjectId[] entityIds = group.GetAllEntityIds();
        
        // Create selection set
        ed.SetImpliedSelection(entityIds);
        
        ed.WriteMessage($"\nSelected {entityIds.Length} entities from group '{groupName}'");
        tr.Commit();
    }
}
```

### 6. Deleting Group

```csharp
[CommandMethod("DELETEGROUP")]
public void DeleteGroup()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for group name
    PromptStringOptions pso = new PromptStringOptions("\nEnter group name to delete: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string groupName = pr.StringResult;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
            OpenMode.ForWrite) as DBDictionary;
        
        if (!groupDict.Contains(groupName))
        {
            ed.WriteMessage($"\nGroup '{groupName}' not found");
            tr.Commit();
            return;
        }
        
        Group group = tr.GetObject(groupDict.GetAt(groupName), 
            OpenMode.ForWrite) as Group;
        
        // Erase the group (entities remain in drawing)
        group.Erase();
        
        ed.WriteMessage($"\nGroup '{groupName}' deleted (entities preserved)");
        tr.Commit();
    }
}
```

## Best Practices

1. **Unique Names**: Use descriptive, unique group names
2. **Selectable Flag**: Set Selectable to true for user interaction
3. **Description**: Provide clear descriptions
4. **Check Membership**: Use Has() before removing entities
5. **Preserve Entities**: Deleting group doesn't delete entities
6. **Anonymous Groups**: Use for temporary grouping

## Related Classes

- [DBDictionary](../Core/DBDictionary.md) - Contains groups
- [ObjectIdCollection](../Collections/ObjectIdCollection.md) - Collection of entity IDs
- [SelectionSet](../EditorInput/SelectionSet.md) - Temporary selection

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-Group)
