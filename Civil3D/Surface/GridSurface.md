# GridSurface Class

## Overview
The `GridSurface` class represents a grid-based surface in Civil3D, an alternative to TIN surfaces using a regular grid of elevation points.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ GridSurface
```

## Key Properties

Inherits all properties from [Surface](Surface.md)

## Code Examples

### Example 1: Accessing Grid Surface
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    
    foreach (ObjectId surfId in surfaceIds)
    {
        Surface surface = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
        
        if (surface is GridSurface gridSurf)
        {
            GeneralSurfaceProperties props = gridSurf.GetGeneralProperties();
            
            ed.WriteMessage($"\nGrid Surface: {gridSurf.Name}");
            ed.WriteMessage($"\nMin Elevation: {props.MinimumElevation:F2}");
            ed.WriteMessage($"\nMax Elevation: {props.MaximumElevation:F2}");
            ed.WriteMessage($"\n2D Area: {props.Area2d:F2}");
        }
    }
    
    tr.Commit();
}
```

## Related Objects
- [Surface](Surface.md) - Base class
- [TinSurface](TinSurface.md) - Alternative surface type
- [CivilDocument](../Core/CivilDocument.md) - Container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-GridSurface)
