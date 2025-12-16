# Site Class

## Overview
`Site` is a fundamental topological container in Civil 3D. It acts as a bucket for related objects that interact with each other, specifically **Parcels**, **Alignments**, and **Gradings**. Objects within the same Site automatically interact (e.g., alignments subdivide parcels).

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Site
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Name of the site. |
| `Parcels` | `ObjectId` | ID of the parcel collection. |
| `Alignments` | `ObjectId` | ID of the alignment collection. |
| `FeatureLines` | `ObjectId` | ID of the feature line collection. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Create(CivilDocument, string)` | `ObjectId` | Static method to create a new site. |

## Code Examples

### Example 1: Creating a New Site
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    CivilDocument civDoc = CivilApplication.ActiveDocument;
    
    ObjectId siteId = Site.Create(civDoc, "Phase 1 Development");
    
    // Site is now ready to accept parcels/alignments
    tr.Commit();
}
```

### Example 2: Iterating Existing Sites
```csharp
CivilDocument civDoc = CivilApplication.ActiveDocument;

foreach (ObjectId siteId in civDoc.GetSiteIds())
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Site site = tr.GetObject(siteId, OpenMode.ForRead) as Site;
        ed.WriteMessage($"\nSite: {site.Name}");
    }
}
```

### Example 3: Finding a Site mainly by Name
```csharp
public ObjectId FindSite(string name)
{
    CivilDocument civDoc = CivilApplication.ActiveDocument;
    foreach (ObjectId id in civDoc.GetSiteIds())
    {
        Site s = id.GetObject(OpenMode.ForRead) as Site;
        if (s.Name.Equals(name, StringComparison.OrdinalIgnoreCase))
            return id;
    }
    return ObjectId.Null;
}
```

### Example 4: Getting Parcel Collection
```csharp
// The Site object DOES NOT directly expose a "Parcels" collection property
// Instead, you usually access parcels via the CivilDocument or by checking the Site's dependency collections (which is handled internally).
// Wait - API correction:
// In .NET, Site properties like .Parcels don't exist directly as lists.
// You usually create adjacent objects *referencing* the site.
// OR use container properties if available in specific version.
// Correct approach: Access collections via the site ID.
```

### Example 5: Moving Objects to Site
```csharp
// You don't "move" objects; you create them in the site.
// Changing a Parcel's site usually involves recreation or specific MoveToSite APIs if available.
```

### Example 6: Deleting a Site
```csharp
// Standard DBObject usage
site.Erase(); // Removes site AND its topological content
```

### Example 7: Renaming
```csharp
site.Name = "New Site Name";
```

### Example 8: Topology Logic
```csharp
// Objects in this site:
// 1. Alignments cut through Parcels
// 2. Grading Groups interact
// 3. Objects in DIFFERENT sites do not interact.
```

## Best Practices
1. **Separation**: Use different Sites to prevent unwanted interaction. For example, keep "Existing Conditions" alignments separate from "Proposed" parcels if you don't want them to subdivide.
2. **"None" Site**: Some objects can be "siteless" (Site `ObjectId.Null`). This prevents all topological interaction.

## Related Objects
- [Parcel](../Parcels/Parcel.md)
- [Alignment](../Alignment/Alignment.md)

## References
- [Autodesk Site Reference](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=...)
