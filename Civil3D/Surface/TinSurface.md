# TinSurface Class

## Overview
The `TinSurface` class represents a Triangulated Irregular Network (TIN) surface in Civil3D, the most common surface type for terrain modeling.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ TinSurface
```

## Key Properties

Inherits all properties from [Surface](Surface.md)

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetTriangles()` | `Triangle[]` | Gets all triangles in the surface |
| `GetPoints()` | `SurfacePoint[]` | Gets all surface points |

## Code Examples

### Example 1: Getting TIN Surface Statistics
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    TinSurface tinSurf = tr.GetObject(surfaceId, OpenMode.ForRead) as TinSurface;
    
    GeneralSurfaceProperties props = tinSurf.GetGeneralProperties();
    
    ed.WriteMessage($"\nTIN Surface: {tinSurf.Name}");
    ed.WriteMessage($"\nNumber of Points: {props.NumberOfPoints}");
    ed.WriteMessage($"\nNumber of Triangles: {props.NumberOfTriangles}");
    ed.WriteMessage($"\nMin Elevation: {props.MinimumElevation:F2}");
    ed.WriteMessage($"\nMax Elevation: {props.MaximumElevation:F2}");
    
    tr.Commit();
}
```

## Related Objects
- [Surface](Surface.md) - Base class
- [GridSurface](GridSurface.md) - Alternative surface type
- [CivilDocument](../Core/CivilDocument.md) - Container

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-TinSurface)
