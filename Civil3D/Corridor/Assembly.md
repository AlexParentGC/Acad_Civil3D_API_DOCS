# Assembly Class

## Overview
The `Assembly` class represents a corridor assembly in Civil3D, which defines the cross-sectional template for a corridor.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Assembly
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the assembly name |
| `Description` | `string` | Gets/sets the description |
| `CodeSetStyleId` | `ObjectId` | Gets/sets the code set style |

## Code Examples

### Example 1: Listing Assemblies
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection assemblyIds = civilDoc.GetAssemblyIds();
    
    ed.WriteMessage($"\nFound {assemblyIds.Count} assemblies:");
    
    foreach (ObjectId assemblyId in assemblyIds)
    {
        Assembly assembly = tr.GetObject(assemblyId, OpenMode.ForRead) as Assembly;
        
        ed.WriteMessage($"\n  {assembly.Name}");
        if (!string.IsNullOrEmpty(assembly.Description))
        {
            ed.WriteMessage($" - {assembly.Description}");
        }
    }
    
    tr.Commit();
}
```

## Related Objects
- [Corridor](Corridor.md) - Uses assemblies for cross-sections
- [CivilDocument](../Core/CivilDocument.md) - Container for assemblies

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Assembly)
