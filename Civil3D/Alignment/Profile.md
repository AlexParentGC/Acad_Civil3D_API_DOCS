# Profile Class

## Overview
The `Profile` class represents a vertical profile along an alignment in Civil3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Profile
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the profile name |
| `Description` | `string` | Gets/sets the description |
| `AlignmentId` | `ObjectId` | Gets the parent alignment ObjectId |
| `StartingStation` | `double` | Gets the starting station |
| `EndingStation` | `double` | Gets the ending station |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `ElevationAt(double)` | `double` | Gets elevation at station |

## Code Examples

### Example 1: Getting Profile Elevation
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    ObjectIdCollection profileIds = alignment.GetProfileIds();
    
    if (profileIds.Count > 0)
    {
        Profile profile = tr.GetObject(profileIds[0], OpenMode.ForRead) as Profile;
        
        double station = 100.0;
        double elevation = profile.ElevationAt(station);
        
        ed.WriteMessage($"\nProfile: {profile.Name}");
        ed.WriteMessage($"\nElevation at Station {station}: {elevation:F2}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Alignment](Alignment.md) - Parent alignment
- [CivilDocument](../Core/CivilDocument.md) - Document container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Profile)
