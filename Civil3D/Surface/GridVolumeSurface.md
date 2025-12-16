# GridVolumeSurface Class

## Overview
The `GridVolumeSurface` class represents a grid-based volume surface in Civil3D, used to calculate cut and fill volumes between two grid surfaces.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ GridVolumeSurface
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `BaseSurfaceId` | `ObjectId` | Gets/sets the base surface |
| `ComparisonSurfaceId` | `ObjectId` | Gets/sets the comparison surface |

## Code Examples

### Example 1: Accessing Grid Volume Surface
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    GridVolumeSurface volSurf = tr.GetObject(volumeSurfaceId, OpenMode.ForRead) as GridVolumeSurface;
    
    ed.WriteMessage($"\nGrid Volume Surface: {volSurf.Name}");
    
    Surface baseSurf = tr.GetObject(volSurf.BaseSurfaceId, OpenMode.ForRead) as Surface;
    Surface compSurf = tr.GetObject(volSurf.ComparisonSurfaceId, OpenMode.ForRead) as Surface;
    
    ed.WriteMessage($"\nBase Surface: {baseSurf.Name}");
    ed.WriteMessage($"\nComparison Surface: {compSurf.Name}");
    
    tr.Commit();
}
```

## Related Objects
- [Surface](Surface.md) - Base class
- [GridSurface](GridSurface.md) - Component surfaces
- [CivilDocument](../Core/CivilDocument.md) - Container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-GridVolumeSurface)
