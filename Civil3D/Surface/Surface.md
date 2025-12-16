# Surface Class

## Overview
The `Surface` class is the base class for all Civil3D surface types including TIN surfaces, grid surfaces, and volume surfaces.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  ├─ TinSurface
                  ├─ GridSurface
                  ├─ TinVolumeSurface
                  └─ GridVolumeSurface
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the surface name |
| `Description` | `string` | Gets/sets the description |
| `StyleId` | `ObjectId` | Gets/sets the surface style |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `FindElevationAtXY(double, double)` | `double` | Gets elevation at XY coordinates |
| `SampleElevations(Point2dCollection)` | `Point3dCollection` | Gets elevations at multiple points |
| `GetGeneralProperties()` | `GeneralSurfaceProperties` | Gets surface statistics |

## Code Examples

### Example 1: Getting Surface Elevation
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    
    if (surfaceIds.Count > 0)
    {
        Surface surface = tr.GetObject(surfaceIds[0], OpenMode.ForRead) as Surface;
        
        double x = 1000.0;
        double y = 2000.0;
        
        try
        {
            double elevation = surface.FindElevationAtXY(x, y);
            ed.WriteMessage($"\nElevation at ({x}, {y}): {elevation:F2}");
        }
        catch
        {
            ed.WriteMessage("\nPoint is outside surface boundary");
        }
    }
    
    tr.Commit();
}
```

### Example 2: Getting Surface Properties
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Surface surface = tr.GetObject(surfaceId, OpenMode.ForRead) as Surface;
    
    GeneralSurfaceProperties props = surface.GetGeneralProperties();
    
    ed.WriteMessage($"\nSurface: {surface.Name}");
    ed.WriteMessage($"\nMin Elevation: {props.MinimumElevation:F2}");
    ed.WriteMessage($"\nMax Elevation: {props.MaximumElevation:F2}");
    ed.WriteMessage($"\n2D Area: {props.Area2d:F2}");
    ed.WriteMessage($"\n3D Area: {props.Area3d:F2}");
    
    tr.Commit();
}
```

## Related Objects
- [TinSurface](TinSurface.md) - Triangulated surface
- [GridSurface](GridSurface.md) - Grid-based surface
- [CivilDocument](../Core/CivilDocument.md) - Container for surfaces

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Surface)
