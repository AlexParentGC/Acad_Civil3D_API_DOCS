# Solid3d Class

## Overview
`Solid3d` represents a 3D volume primitive or ACIS solid in the AutoCAD database. It is the primary class for creating complex 3D geometry using Boolean operations (Union, Subtract, Intersect) and primitives (Box, Sphere, Cylinder, etc.).

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Solid3d
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Mass` | `double` | Volume of the solid. |
| `Area` | `double` | Surface area. |
| `Centroid` | `Point3d` | Center of mass. |
| `MomentsOfInertia` | `Vector3d` | Moments of inertia. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `CreateBox(double, double, double)` | `void` | Creates a box solid. |
| `CreateSphere(double)` | `void` | Creates a sphere solid. |
| `CreateCylinder(double, double)` | `void` | Creates a cylinder. |
| `CreateExtrudedSolid(Entity, Vector3d, SweepOptions)` | `void` | Extrudes a profile. |
| `BooleanOperation(BooleanOperationType, Solid3d)` | `void` | Performs Union/Subtract/Intersect. |
| `Slice(Plane, bool)` | `Solid3d` | Slices the solid by a plane. |

## Code Examples

### Example 1: Creating a Simple Box
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Solid3d box = new Solid3d();
    box.CreateBox(10.0, 10.0, 10.0); // 10x10x10 cube
    
    // Position it at 5,5,5
    box.TransformBy(Matrix3d.Displacement(new Vector3d(5, 5, 5)));
    
    btr.AppendEntity(box);
    tr.AddNewlyCreatedDBObject(box, true);
    
    tr.Commit();
}
```

### Example 2: Boolean subtract (Hollow Box)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Outer box
    Solid3d outer = new Solid3d();
    outer.CreateBox(12, 12, 12);
    
    // Inner box
    Solid3d inner = new Solid3d();
    inner.CreateBox(10, 10, 10);
    
    // Use boolean subtraction: Outer - Inner
    outer.BooleanOperation(BooleanOperationType.BoolSubtract, inner);
    
    // Result is a hollow box (if centered)
    // Note: 'inner' is consumed/invalidated by the operation unless you cloned it,
    // but here we just dispose it implicitly or explicitly.
    // Actually, BooleanOperation typically destroys the second operand.
    // You only add 'outer' to the database.
    inner.Dispose();
    
    btr.AppendEntity(outer);
    tr.AddNewlyCreatedDBObject(outer, true);
    
    tr.Commit();
}
```

### Example 3: Extruding a Circle (Cylinder)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create Profile
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    
    // Create Solid
    Solid3d cylinder = new Solid3d();
    cylinder.CreateExtrudedSolid(circle, Vector3d.ZAxis * 10, new SweepOptions());
    
    btr.AppendEntity(cylinder);
    tr.AddNewlyCreatedDBObject(cylinder, true);
    
    tr.Commit();
}
```

### Example 4: Getting Mass Properties
```csharp
public void LogMassProps(Solid3d solid)
{
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage(
        $"\nVolume: {solid.Mass:F2}" +
        $"\nArea: {solid.Area:F2}" +
        $"\nCentroid: {solid.Centroid}"
    );
}
```

### Example 5: Slicing a Solid
```csharp
Solid3d solid = ...;
Plane slicePlane = new Plane(Point3d.Origin, Vector3d.XAxis); // Slice at X=0

// Perform slice
// solid is modified to be the "negative" side of the plane
// Returns the "positive" side as a new Solid3d
Solid3d otherHalf = solid.Slice(slicePlane, negativeHalfToo: true);

if (otherHalf != null)
{
    // Add other half to database
}
```

### Example 6: Checking for Interference
```csharp
Solid3d sol1 = ...;
Solid3d sol2 = ...;

bool interference = sol1.CheckInterference(sol2);

if (interference)
{
    // Create interference solid
    Solid3d intersection = sol1.Clone() as Solid3d;
    intersection.BooleanOperation(BooleanOperationType.BoolIntersect, sol2);
    // 'intersection' is now the common volume
}
```

### Example 7: Creating from Region (Revolve)
```csharp
Region reg = ...; // 2D Profile
Solid3d revSolid = new Solid3d();

// Revolve region 360 degrees around Y axis
revSolid.CreateRevolvedSolid(reg, Point3d.Origin, Vector3d.YAxis, Math.PI * 2, 0.0, new RevolveOptions());
```

### Example 8: Chamfering Edges (Brep)
```csharp
// Advanced: ACIS edge manipulation often requires Brep API or specific Solid3d methods
// Simple modifications usually done via Boolean ops.
// Direct edge chamfering via API is complex and often involves accessing the internal BRep.
```

## Best Practices
1. **Disposal**: `Solid3d` objects consume significant unmanaged memory (ACIS). Always `Dispose()` them if created in memory and not added to the database.
2. **Boolean Ops**: The second operand in `BooleanOperation` is essentially destroyed/consumed. Do not try to reuse it after the operation.
3. **Mass Props**: Calculating mass properties (`Mass`, `Centroid`) can be computationally expensive for complex solids. Cache values if needed.
4. **History**: Solid history (recording steps) can bloat file size. Check `RecordHistory` property.

## Related Objects
- [Region](Region.md) - 2D equivalent.
- [Body](Body.md) - Wireframe/Sheet body wrapper.
- [Region](../3D/Region.md)

## References
- [Autodesk Solid3d Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Solid3d)
