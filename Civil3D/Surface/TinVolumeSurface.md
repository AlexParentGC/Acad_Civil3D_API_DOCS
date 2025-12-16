# TinVolumeSurface Class

## Overview
The `TinVolumeSurface` class represents a TIN-based volume surface in Civil3D, used to calculate cut and fill volumes between two surfaces.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ TinVolumeSurface
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `BaseSurfaceId` | `ObjectId` | Gets/sets the base surface |
| `ComparisonSurfaceId` | `ObjectId` | Gets/sets the comparison surface |

## Code Examples

### Example 1: Getting Volume Surface Information
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    TinVolumeSurface volSurf = tr.GetObject(volumeSurfaceId, OpenMode.ForRead) as TinVolumeSurface;
    
    ed.WriteMessage($"\nVolume Surface: {volSurf.Name}");
    
    // Get base and comparison surfaces
    Surface baseSurf = tr.GetObject(volSurf.BaseSurfaceId, OpenMode.ForRead) as Surface;
    Surface compSurf = tr.GetObject(volSurf.ComparisonSurfaceId, OpenMode.ForRead) as Surface;
    
    ed.WriteMessage($"\nBase Surface: {baseSurf.Name}");
    ed.WriteMessage($"\nComparison Surface: {compSurf.Name}");
    
    tr.Commit();
}
```

## Related Objects
- [Surface](Surface.md) - Base class
- [TinSurface](TinSurface.md) - Component surfaces
- [CivilDocument](../Core/CivilDocument.md) - Container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-TinVolumeSurface)
