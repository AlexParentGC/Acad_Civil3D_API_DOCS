# NurbSurface Class

## Overview
The `NurbSurface` class represents a generic NURB (Non-Uniform Rational B-Spline) parametric surface in 3D space. NURBS surfaces are the 3D extension of NURBS curves, providing precise mathematical representation of complex freeform surfaces.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ControlPoints` | `Point3d[,]` | Gets the 2D grid of control points |
| `UKnots` | `KnotCollection` | Gets the knot vector in U direction |
| `VKnots` | `KnotCollection` | Gets the knot vector in V direction |
| `UWeights` | `DoubleCollection` | Gets weights in U direction |
| `VWeights` | `DoubleCollection` | Gets weights in V direction |
| `UDegree` | `int` | Gets polynomial degree in U direction |
| `VDegree` | `int` | Gets polynomial degree in V direction |
| `NumControlPointsInU` | `int` | Gets number of control points in U |
| `NumControlPointsInV` | `int` | Gets number of control points in V |
| `IsRational` | `bool` | Checks if surface has weights |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `EvaluatePoint(double, double)` | `Point3d` | Evaluates point at (u,v) parameters |
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on surface |
| `GetNormalAt(double, double)` | `Vector3d` | Gets surface normal at (u,v) |
| `GetDerivativesAt(double, double)` | `Vector3d[]` | Gets partial derivatives at (u,v) |

## Code Examples

### Example 1: Creating NURBS Surface
```csharp
// Create 4x4 control point grid
int uCount = 4;
int vCount = 4;
Point3d[,] controlPoints = new Point3d[uCount, vCount];

for (int i = 0; i < uCount; i++)
{
    for (int j = 0; j < vCount; j++)
    {
        controlPoints[i, j] = new Point3d(i * 10, j * 10, Math.Sin(i) * Math.Cos(j) * 5);
    }
}

// Define knot vectors (degree 3)
DoubleCollection uKnots = new DoubleCollection();
uKnots.Add(0); uKnots.Add(0); uKnots.Add(0); uKnots.Add(0);
uKnots.Add(1); uKnots.Add(1); uKnots.Add(1); uKnots.Add(1);

DoubleCollection vKnots = new DoubleCollection();
vKnots.Add(0); vKnots.Add(0); vKnots.Add(0); vKnots.Add(0);
vKnots.Add(1); vKnots.Add(1); vKnots.Add(1); vKnots.Add(1);

NurbSurface surface = new NurbSurface(3, 3, uKnots, vKnots, controlPoints, false);

ed.WriteMessage($"\nNURBS Surface created");
ed.WriteMessage($"\nU degree: {surface.UDegree}, V degree: {surface.VDegree}");
ed.WriteMessage($"\nControl points: {surface.NumControlPointsInU} x {surface.NumControlPointsInV}");
```

### Example 2: Evaluating Points on Surface
```csharp
NurbSurface surface = /* existing NURBS surface */;

// Sample the surface
for (double u = 0; u <= 1.0; u += 0.25)
{
    for (double v = 0; v <= 1.0; v += 0.25)
    {
        Point3d pt = surface.EvaluatePoint(u, v);
        Vector3d normal = surface.GetNormalAt(u, v);
        
        ed.WriteMessage($"\nAt (u={u:F2}, v={v:F2}): Point={pt}, Normal={normal}");
    }
}
```

### Example 3: Finding Closest Point on Surface
```csharp
NurbSurface surface = /* existing surface */;
Point3d testPoint = new Point3d(15, 15, 10);

Point3d closestPoint = surface.GetClosestPointTo(testPoint);
double distance = testPoint.DistanceTo(closestPoint);

ed.WriteMessage($"\nTest point: {testPoint}");
ed.WriteMessage($"\nClosest point on surface: {closestPoint}");
ed.WriteMessage($"\nDistance: {distance:F4}");
```

## Best Practices

1. **Control Point Grid**: Must be rectangular (m x n grid)
2. **Knot Vectors**: U knots length = numU + degreeU + 1, V knots length = numV + degreeV + 1
3. **Parameters**: U and V parameters typically range from 0 to 1
4. **Normals**: Surface normal points perpendicular to surface
5. **Rational**: Use weights for more control over surface shape
6. **Evaluation**: Sample surface with sufficient (u,v) points for visualization

## Related Classes
- **NurbCurve3d** - NURBS curve (1D)
- **BoundedPlane** - Planar surface
- **Point3d** - Control points and evaluated points
- **Vector3d** - Surface normals

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [NurbSurface Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_NurbSurface)
- [NURBS Surfaces on Wikipedia](https://en.wikipedia.org/wiki/Non-uniform_rational_B-spline#NURBS_surfaces)
