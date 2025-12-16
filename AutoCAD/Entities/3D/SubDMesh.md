# SubDMesh Class

## Overview
`SubDMesh` (Subdivision Mesh) represents a modern mesh object capable of smoothing, creasing, and complex organic modeling. Unlike legacy `PolygonMesh`, it supports n-gon faces, varying smoothness levels, and edge/vertex weighting.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ SubDMesh
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `SubdivisionLevel` | `int` | Current smoothness level (0=unsmoothed). |
| `VertexCount` | `int` | Number of vertices. |
| `FaceCount` | `int` | Number of faces. |
| `Volume` | `double` | Volume (if watertight). |
| `SurfaceArea` | `double` | Surface area. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SetSubdivisionLevel(int)` | `void` | Sets smoothness. |
| `SetBox(...)` | `void` | Creates a mesh box. |
| `SetSphere(...)` | `void` | Creates a mesh sphere. |
| `SubdivideFace(int)` | `void` | Refines a specific face. |
| `ExtrudeFaces(...)` | `void` | Extrudes faces. |

## Code Examples

### Example 1: Creating a Mesh Box
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    SubDMesh mesh = new SubDMesh();
    // 10x10x10 box with 4x4x4 subdivisions
    mesh.SetBox(10, 10, 10, 4, 4, 4);
    
    btr.AppendEntity(mesh);
    tr.AddNewlyCreatedDBObject(mesh, true);
    
    tr.Commit();
}
```

### Example 2: Smoothing a Mesh
```csharp
mesh.SetSubdivisionLevel(2); // Smooths the mesh geometry
```

### Example 3: Defining Custom Mesh (Vertices/Faces)
```csharp
Point3dCollection vertices = new Point3dCollection();
vertices.Add(new Point3d(0,0,0));
vertices.Add(new Point3d(10,0,0));
vertices.Add(new Point3d(0,10,0));
// ...

Int32Collection faceCounts = new Int32Collection();
faceCounts.Add(3); // Triangle

Int32Collection faceIndices = new Int32Collection();
faceIndices.Add(0); faceIndices.Add(1); faceIndices.Add(2); // Vertex indices

SubDMesh mesh = new SubDMesh();
mesh.SetSubDMesh(vertices, faceIndices, faceCounts.Count);
```

### Example 4: Converting to Solid
```csharp
// SubDMesh can be converted to Solid3d if it encloses a volume
if (mesh.Watertight)
{
    Solid3d sol = mesh.ConvertToSolid(false, false);
    // Add sol to database...
}
```

### Example 5: Surface Conversion
```csharp
// Convert to Surface (NURBS)
Autodesk.AutoCAD.DatabaseServices.Surface surf = mesh.ConvertToSurface(false, false);
```

### Example 6: Iterating Vertices
```csharp
// Access vertices directly
// Warning: This can be large
foreach (Point3d pt in mesh.Vertices)
{
    // ...
}
```

### Example 7: Creasing Edges
```csharp
// Apply crease to maintain sharp edges during smoothing
FullSubentityPath[] edges = ...; // Need to identify edges
mesh.SetCrease(edges, 1.0); // Full crease
```

### Example 8: Extruding a Face
```csharp
// Advanced: Extruding a subentity face
FullSubentityPath facePath = ...;
mesh.ExtrudeFaces(new[] { facePath }, 5.0, 0.0);
```

## Best Practices
1. **Watertight Check**: Check `Watertight` property before attempting volume calculations or solid conversion.
2. **Level of Detail**: High `SubdivisionLevel` exponentially increases vertex count. Keep it low for performance.
3. **Subentities**: Manipulating individual faces/edges requires identifying them via `GetSubentPathsAtGsMarker` or similar selection mechanisms.

## Related Objects
- [Solid3d](Solid3d.md)
- [PolygonMesh](../Geometric/PolygonMesh.md) (Legacy)

## References
- [Autodesk SubDMesh Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_SubDMesh)
