# MLeader Class

## Overview
The `MLeader` class represents a modern multi-leader annotation entity in AutoCAD (introduced in AutoCAD 2008). MLeaders are more flexible than legacy Leaders, supporting multiple leader lines, various content types (text, blocks, or none), and dedicated MLeader styles for consistent formatting.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ MLeader
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ContentType` | `ContentType` | Gets/sets content type (MText, Block, None) |
| `MText` | `MText` | Gets/sets the MText content |
| `BlockContentId` | `ObjectId` | Gets/sets the block content |
| `MLeaderStyle` | `ObjectId` | Gets/sets the MLeader style |
| `ArrowSymbolId` | `ObjectId` | Gets/sets the arrowhead block |
| `LeaderLineType` | `LeaderType` | Gets/sets leader line type (Straight, Spline) |
| `TextAttachmentType` | `TextAttachmentType` | Gets/sets how text attaches to leader |
| `DoglegLength` | `double` | Gets/sets the horizontal landing length |
| `EnableDogleg` | `bool` | Gets/sets whether dogleg is enabled |
| `EnableLanding` | `bool` | Gets/sets whether landing line is enabled |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `AddLeader()` | `int` | Adds a new leader line and returns its index |
| `RemoveLeader(int)` | `void` | Removes the leader at the specified index |
| `AddLeaderLine(int)` | `int` | Adds a leader line to an existing leader |
| `RemoveLeaderLine(int, int)` | `void` | Removes a leader line |
| `SetFirstVertex(int, Point3d)` | `void` | Sets the first vertex of a leader line |
| `SetLastVertex(int, Point3d)` | `void` | Sets the last vertex of a leader line |
| `GetLeaderIndex(int)` | `int` | Gets the leader index for a leader line |
| `GetLeaderLineIndexes(int)` | `IntegerCollection` | Gets all leader line indexes for a leader |

## Code Examples

### Example 1: Creating a Simple MLeader with Text
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create MLeader
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    
    // Set content type to MText
    mleader.ContentType = ContentType.MTextContent;
    
    // Set text content
    MText mtext = new MText();
    mtext.Contents = "This is an MLeader note";
    mleader.MText = mtext;
    
    // Add leader line
    int leaderIndex = mleader.AddLeader();
    int lineIndex = mleader.AddLeaderLine(leaderIndex);
    
    // Set vertices (from text to feature)
    mleader.SetFirstVertex(lineIndex, new Point3d(10, 10, 0));
    mleader.SetLastVertex(lineIndex, new Point3d(0, 0, 0));
    
    // Add to drawing
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Example 2: Creating MLeader with Multiple Leader Lines
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    mleader.ContentType = ContentType.MTextContent;
    
    MText mtext = new MText();
    mtext.Contents = "Multiple Leaders";
    mleader.MText = mtext;
    
    // Add first leader
    int leader1 = mleader.AddLeader();
    int line1 = mleader.AddLeaderLine(leader1);
    mleader.SetFirstVertex(line1, new Point3d(10, 10, 0));
    mleader.SetLastVertex(line1, new Point3d(0, 0, 0));
    
    // Add second leader
    int leader2 = mleader.AddLeader();
    int line2 = mleader.AddLeaderLine(leader2);
    mleader.SetFirstVertex(line2, new Point3d(10, 10, 0));
    mleader.SetLastVertex(line2, new Point3d(5, -5, 0));
    
    // Add third leader
    int leader3 = mleader.AddLeader();
    int line3 = mleader.AddLeaderLine(leader3);
    mleader.SetFirstVertex(line3, new Point3d(10, 10, 0));
    mleader.SetLastVertex(line3, new Point3d(-5, 5, 0));
    
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Example 3: Creating MLeader with Block Content
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    
    // Set content type to Block
    mleader.ContentType = ContentType.BlockContent;
    
    // Set block content (assuming "DetailTag" block exists)
    if (bt.Has("DetailTag"))
    {
        mleader.BlockContentId = bt["DetailTag"];
    }
    
    // Add leader
    int leaderIndex = mleader.AddLeader();
    int lineIndex = mleader.AddLeaderLine(leaderIndex);
    mleader.SetFirstVertex(lineIndex, new Point3d(10, 10, 0));
    mleader.SetLastVertex(lineIndex, new Point3d(0, 0, 0));
    
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Example 4: Setting MLeader Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForWrite) as MLeader;
    
    // Set leader line type
    mleader.LeaderLineType = LeaderType.SplineLeader;
    
    // Enable and set dogleg
    mleader.EnableDogleg = true;
    mleader.DoglegLength = 5.0;
    
    // Enable landing
    mleader.EnableLanding = true;
    
    // Set text attachment
    mleader.TextAttachmentType = TextAttachmentType.AttachmentMiddle;
    
    // Set color
    mleader.ColorIndex = 3; // Green
    
    tr.Commit();
}
```

### Example 5: Reading MLeader Information
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForRead) as MLeader;
    
    ed.WriteMessage("\n=== MLeader Information ===");
    ed.WriteMessage($"\nContent Type: {mleader.ContentType}");
    ed.WriteMessage($"\nLeader Line Type: {mleader.LeaderLineType}");
    ed.WriteMessage($"\nDogleg Enabled: {mleader.EnableDogleg}");
    ed.WriteMessage($"\nDogleg Length: {mleader.DoglegLength:F2}");
    ed.WriteMessage($"\nLanding Enabled: {mleader.EnableLanding}");
    
    if (mleader.ContentType == ContentType.MTextContent)
    {
        ed.WriteMessage($"\nText Content: {mleader.MText.Contents}");
    }
    
    // Count leaders
    int leaderCount = 0;
    for (int i = 0; i < 100; i++) // Arbitrary upper limit
    {
        try
        {
            IntegerCollection lines = mleader.GetLeaderLineIndexes(i);
            if (lines != null && lines.Count > 0)
                leaderCount++;
            else
                break;
        }
        catch
        {
            break;
        }
    }
    
    ed.WriteMessage($"\nNumber of Leaders: {leaderCount}");
    
    tr.Commit();
}
```

### Example 6: Applying MLeader Style
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForWrite) as MLeader;
    
    // Get MLeader style dictionary
    DBDictionary mleaderDict = tr.GetObject(db.MLeaderStyleDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (mleaderDict.Contains("Annotative"))
    {
        mleader.MLeaderStyle = mleaderDict.GetAt("Annotative");
        ed.WriteMessage("\nApplied 'Annotative' MLeader style");
    }
    
    tr.Commit();
}
```

### Example 7: Creating MLeader with No Content
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create MLeader with no content (just leader lines)
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    mleader.ContentType = ContentType.NoneContent;
    
    // Add leader
    int leaderIndex = mleader.AddLeader();
    int lineIndex = mleader.AddLeaderLine(leaderIndex);
    mleader.SetFirstVertex(lineIndex, new Point3d(10, 0, 0));
    mleader.SetLastVertex(lineIndex, new Point3d(0, 0, 0));
    
    mleader.ColorIndex = 1; // Red
    
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Example 8: Modifying MLeader Text
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForWrite) as MLeader;
    
    if (mleader.ContentType == ContentType.MTextContent)
    {
        MText mtext = mleader.MText;
        mtext.Contents = "Updated text content\\PWith multiple lines";
        mtext.TextHeight = 3.0;
        mleader.MText = mtext;
        
        ed.WriteMessage("\nUpdated MLeader text");
    }
    
    tr.Commit();
}
```

## Content Types

| Type | Description |
|------|-------------|
| `MTextContent` | Multi-line text annotation |
| `BlockContent` | Block reference annotation |
| `NoneContent` | No content (leader lines only) |

## Leader Types

| Type | Description |
|------|-------------|
| `StraightLeader` | Straight line segments |
| `SplineLeader` | Smooth spline curve |

## Text Attachment Types

| Type | Description |
|------|-------------|
| `AttachmentTop` | Attach to top of text |
| `AttachmentMiddle` | Attach to middle of text |
| `AttachmentBottom` | Attach to bottom of text |

## MLeader vs Leader

| Feature | Leader (Legacy) | MLeader (Modern) |
|---------|----------------|------------------|
| Introduction | R13 | 2008 |
| Multiple Leaders | No | Yes |
| Content Types | Limited | Text, Block, None |
| Styles | Dimension Style | MLeader Style |
| Flexibility | Basic | Advanced |
| Recommended | Legacy only | All new work |

## Best Practices

1. **Use MLeader Styles**: Create and use MLeader styles for consistency
2. **Content Type**: Choose appropriate content type for your needs
3. **Multiple Leaders**: Use for pointing to multiple features
4. **Dogleg**: Enable for horizontal landing line
5. **Spline Leaders**: Use for organic, curved appearance
6. **Text Formatting**: Use MText formatting codes for rich text
7. **Block Content**: Use for standardized callouts and tags
8. **Layer Management**: Place MLeaders on appropriate annotation layers

## Common Use Cases

- **Callouts**: Multi-point feature identification
- **Notes**: Explanatory annotations
- **Detail Tags**: Standardized detail references
- **Part Labels**: Component identification
- **Revision Clouds**: Revision annotations
- **Weld Symbols**: Welding annotations

## Related Objects
- [Leader](Leader.md) - Legacy leader annotation
- [Dimension](Dimension.md) - Dimension annotation base class
- [Entity](../../BaseClasses/Entity.md) - Base class for all entities
- MText - Text content for MLeaders
- BlockReference - Block content for MLeaders
- MLeaderStyle - Style definition for MLeaders

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [MLeader Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_MLeader)
