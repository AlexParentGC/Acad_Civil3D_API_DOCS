# Region Class

## Overview
`Region` represents a bounded planar area with physical properties (Area, Perimeter, Centroid, Inertia). Regions are created from closed loops of curves (lines, arcs, polylines) and form the basis for creating complex 2D shapes that can be extruded into `Solid3d` objects.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Region
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Area` | `double` | Total area of the region. |
| `Perimeter` | `double` | Total length of boundaries. |
| `Normal` | `Vector3d` | Normal vector of the region plane. |
| `AreaProperties` | `RegionAreaProperties` | Complex physical properties. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `CreateFromCurves(DBObjectCollection)` | `DBObjectCollection` | Static method to create regions from curves. |
| `BooleanOperation(BooleanOperationType, Region)` | `void` | Union, Subtract, Intersect. |
| `GetAreaProp(Point3d, Vector3d, Vector3d)` | `void` | Calculates area properties relative to axes. |

## Code Examples

### Example 1: Creating a Region from a Circle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Define input curve
    Circle circ = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    DBObjectCollection curves = new DBObjectCollection();
    curves.Add(circ);
    
    // Create Region
    DBObjectCollection regions = Region.CreateFromCurves(curves);
    
    if (regions.Count > 0)
    {
        Region reg = regions[0] as Region;
        btr.AppendEntity(reg);
        tr.AddNewlyCreatedDBObject(reg, true);
    }
    
    // Dispose collection (but NOT the region we added to DB)
    regions.Dispose();
    tr.Commit();
}
```

### Example 2: Boolean Intersection
```csharp
// Two overlapping regions
Region r1 = ...;
Region r2 = ...;

// Intersect: Result is the common area
r1.BooleanOperation(BooleanOperationType.BoolIntersect, r2);

// r1 is now the intersection
// r2 is invalidated
r2.Dispose();
```

### Example 3: Calculating Area
```csharp
public void ShowArea(Region reg)
{
    ed.WriteMessage($"\nArea: {reg.Area}");
    ed.WriteMessage($"\nPerimeter: {reg.Perimeter}");
}
```

### Example 4: Creating Complex Shape (Subtracted)
```csharp
// Create square
DBObjectCollection p1 = new DBObjectCollection();
p1.Add(new Polyline(4)); // ... assume 10x10 square ...
Region outer = Region.CreateFromCurves(p1)[0] as Region;

// Create circle
DBObjectCollection p2 = new DBObjectCollection();
p2.Add(new Circle(center, normal, 2.0));
Region inner = Region.CreateFromCurves(p2)[0] as Region;

// Subtract hole from square
outer.BooleanOperation(BooleanOperationType.BoolSubtract, inner);
inner.Dispose();

// Result: Square with a hole
```

### Example 5: Exploding a Region
```csharp
DBObjectCollection parts = new DBObjectCollection();
region.Explode(parts);

// Result is usually the boundary curves (Lines, Arcs, Splines)
foreach (Entity e in parts)
{
    // ...
}
```

### Example 6: Checking Coplanarity (Prerequisite)
```csharp
// Region.CreateFromCurves fails if curves are not coplanar
// or not closed.
// Always validate input curves.
```

### Example 7: Extruding to Solid
```csharp
Region reg = ...;
Solid3d sol = new Solid3d();
sol.CreateExtrudedSolid(reg, Vector3d.ZAxis * 5.0, new SweepOptions());
```

### Example 8: Mass Properties Analysis
```csharp
Plane plane = new Plane(Point3d.Origin, Vector3d.ZAxis);
Solid3dMassProperties props = reg.GetMassProperties();

ed.WriteMessage($"\nCentroid: {props.Centroid}");
ed.WriteMessage($"\nMoments: {props.PrincipalMoments}");
```

## Best Practices
1. **Input Validation**: `CreateFromCurves` is strict. Curves must form a closed loop, be planar, and not self-intersect.
2. **Disposal**: Like `Solid3d`, Regions wrap native ACIS entities. Dispose if not persistent.
3. **Boolean Ops**: Operands must be on the same plane.
4. **Collections**: `CreateFromCurves` returns a collection because one set of curves might result in multiple disjoint regions (e.g., two circles). Handle all returned regions.

## Related Objects
- [Solid3d](Solid3d.md) - 3D equivalent.
- [Hatch](../Complex/Hatch.md) - Often uses boundaries similar to Regions.

## References
- [Autodesk Region Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Region)
