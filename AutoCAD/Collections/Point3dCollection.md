# Point3dCollection Class

## Overview
`Point3dCollection` is a specialized collection for `Point3d` structures. It is optimized for geometry operations and is required by many API methods involving curves, 3D solids, and polygon meshes (e.g., `GetSplitCurves`, `CreateFromCurves`).

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Count` | `int` | Number of points. |
| `this[int]` | `Point3d` | Indexer for accessing points. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Add(Point3d)` | `int` | Adds a point. |
| `Insert(int, Point3d)` | `void` | Inserts point at index. |
| `Remove(Point3d)` | `void` | Removes specific point. |
| `RemoveAt(int)` | `void` | Removes point at index. |
| `Clear()` | `void` | Empty the list. |
| `ToArray()` | `Point3d[]` | Export simple array. |

## Code Examples

### Example 1: Basic Usage
```csharp
Point3dCollection pts = new Point3dCollection();
pts.Add(new Point3d(0, 0, 0));
pts.Add(new Point3d(10, 0, 0));
pts.Add(new Point3d(10, 10, 0));

ed.WriteMessage($"\nDefined polygon with {pts.Count} vertices.");
```

### Example 2: Creating a 3D Polyline
```csharp
Point3dCollection vertices = new Point3dCollection();
// ... fill vertices ...

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = new Polyline3d(Poly3dType.SimplePoly, vertices, false);
    // ... add to database ...
}
```

### Example 3: Defining a Selection Fence
```csharp
// Selection fences often require Point3dCollection
Point3dCollection fence = new Point3dCollection();
fence.Add(p1);
fence.Add(p2);
fence.Add(p3);

PromptSelectionResult res = ed.SelectFence(fence);
```

### Example 4: Intersecting Entities
```csharp
Entity ent1 = ...;
Entity ent2 = ...;

Point3dCollection intersectPts = new Point3dCollection();

// IntersectWith populates the collection
ent1.IntersectWith(ent2, Intersect.OnBothOperands, intersectPts, IntPtr.Zero, IntPtr.Zero);

foreach (Point3d pt in intersectPts)
{
    ed.WriteMessage($"\nIntersection at: {pt}");
}
```

### Example 5: Bulk Initialization
```csharp
Point3d[] rawPoints = { pt1, pt2, pt3 };
Point3dCollection col = new Point3dCollection(rawPoints);
```

### Example 6: Checking for Duplicate Points
```csharp
// Point3dCollection doesn't automatically filter duplicates
if (!pts.Contains(newPoint))
{
    pts.Add(newPoint);
}
```

### Example 7: Copying
```csharp
Point3dCollection copy = new Point3dCollection();
foreach (Point3d pt in original)
{
    copy.Add(pt);
}
```

### Example 8: Disposing
```csharp
// Point3dCollection implements IDisposable
using (Point3dCollection pts = new Point3dCollection())
{
    // Use collection
} // Disposed
```

## Best Practices
1. **Dispose**: It implements `IDisposable`. Always use `using` or call `Dispose()` to free unmanaged resources associated with the underlying geometries.
2. **Capacity**: Unlike generic Lists, it doesn't expose Capacity control easily, but is generally efficient.
3. **Array Conversion**: Use `ToArray()` if you need to pass data to non-AutoCAD .NET methods.

## Related Objects
- [Point3d](../Geometry/Point3d.md) - The item type.
- [ObjectIdCollection](ObjectIdCollection.md) - Collection of IDs.

## References
- [Autodesk Point3dCollection Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Point3dCollection)
