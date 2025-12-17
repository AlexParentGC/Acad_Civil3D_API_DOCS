# NurbCurve3d Class

## Overview
The `NurbCurve3d` class represents a non-uniform rational B-spline (NURBS) curve in 3D space. NURBS curves are fundamental to CAD systems, providing precise mathematical representations of complex freeform curves using control points, knots, and weights.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ControlPoints` | `Point3dCollection` | Gets the control points that define the curve shape |
| `Knots` | `KnotCollection` | Gets the knot vector that determines parameter values |
| `Weights` | `DoubleCollection` | Gets the weights for rational NURBS (control point influence) |
| `Degree` | `int` | Gets the polynomial degree of the curve |
| `NumControlPoints` | `int` | Gets the number of control points |
| `NumKnots` | `int` | Gets the number of knots in the knot vector |
| `NumWeights` | `int` | Gets the number of weights (0 for non-rational) |
| `IsPeriodic` | `bool` | Checks if the curve is periodic (closed) |
| `IsRational` | `bool` | Checks if the curve is rational (has weights) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `EvaluatePoint(double)` | `Point3d` | Evaluates point on curve at parameter value |
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on curve to given point |
| `GetDistanceTo(Point3d)` | `double` | Gets distance from point to curve |
| `AddControlPointAt(int, Point3d)` | `void` | Adds control point at specified index |
| `DeleteControlPointAt(int)` | `void` | Removes control point at index |
| `GetWeightAt(int)` | `double` | Gets weight at specified index |
| `SetWeightAt(int, double)` | `void` | Sets weight at specified index |
| `InsertKnot(double)` | `void` | Inserts knot at parameter value |
| `AddKnot(double)` | `void` | Adds knot to knot vector |
| `ElevateDegree(int)` | `void` | Increases curve degree without changing shape |
| `GetDerivativeAt(double)` | `Vector3d` | Gets derivative (tangent) at parameter |

## Code Examples

### Example 1: Creating a NURBS Curve
```csharp
// Define control points
Point3dCollection controlPoints = new Point3dCollection();
controlPoints.Add(new Point3d(0, 0, 0));
controlPoints.Add(new Point3d(10, 5, 0));
controlPoints.Add(new Point3d(20, 5, 0));
controlPoints.Add(new Point3d(30, 0, 0));

// Define knot vector (degree 3, 4 control points = 8 knots)
DoubleCollection knots = new DoubleCollection();
knots.Add(0); knots.Add(0); knots.Add(0); knots.Add(0);
knots.Add(1); knots.Add(1); knots.Add(1); knots.Add(1);

// Create NURBS curve (degree 3)
int degree = 3;
NurbCurve3d nurbsCurve = new NurbCurve3d(degree, knots, controlPoints, false);

ed.WriteMessage($"\nNURBS Curve created");
ed.WriteMessage($"\nDegree: {nurbsCurve.Degree}");
ed.WriteMessage($"\nControl Points: {nurbsCurve.NumControlPoints}");
ed.WriteMessage($"\nKnots: {nurbsCurve.NumKnots}");
```

### Example 2: Evaluating Points on NURBS Curve
```csharp
NurbCurve3d curve = /* existing NURBS curve */;

// Evaluate points along the curve
int numSamples = 10;
for (int i = 0; i <= numSamples; i++)
{
    double param = i / (double)numSamples;
    Point3d pt = curve.EvaluatePoint(param);
    
    ed.WriteMessage($"\nParameter {param:F2}: ({pt.X:F2}, {pt.Y:F2}, {pt.Z:F2})");
}

// Get tangent at midpoint
double midParam = 0.5;
Vector3d tangent = curve.GetDerivativeAt(midParam);
ed.WriteMessage($"\nTangent at 0.5: {tangent}");
```

### Example 3: Creating Weighted (Rational) NURBS
```csharp
Point3dCollection controlPoints = new Point3dCollection();
controlPoints.Add(new Point3d(0, 0, 0));
controlPoints.Add(new Point3d(10, 10, 0));
controlPoints.Add(new Point3d(20, 10, 0));
controlPoints.Add(new Point3d(30, 0, 0));

// Define weights (higher weight pulls curve closer to control point)
DoubleCollection weights = new DoubleCollection();
weights.Add(1.0);  // Normal weight
weights.Add(2.0);  // Double weight - pulls curve toward this point
weights.Add(2.0);  // Double weight
weights.Add(1.0);  // Normal weight

DoubleCollection knots = new DoubleCollection();
knots.Add(0); knots.Add(0); knots.Add(0); knots.Add(0);
knots.Add(1); knots.Add(1); knots.Add(1); knots.Add(1);

// Create rational NURBS curve
NurbCurve3d rationalCurve = new NurbCurve3d(3, knots, controlPoints, weights, false);

ed.WriteMessage($"\nRational NURBS: {rationalCurve.IsRational}");
ed.WriteMessage($"\nWeights: {rationalCurve.NumWeights}");
```

### Example 4: Modifying NURBS Curve
```csharp
NurbCurve3d curve = /* existing curve */;

// Add a control point
Point3d newControlPoint = new Point3d(15, 8, 0);
curve.AddControlPointAt(2, newControlPoint);

// Modify a weight
if (curve.IsRational)
{
    curve.SetWeightAt(1, 3.0); // Increase weight at index 1
}

// Insert a knot to add detail
curve.InsertKnot(0.5);

// Elevate degree for smoother curve
curve.ElevateDegree(4);

ed.WriteMessage($"\nModified curve:");
ed.WriteMessage($"\nControl Points: {curve.NumControlPoints}");
ed.WriteMessage($"\nDegree: {curve.Degree}");
```

### Example 5: Converting NURBS to Polyline
```csharp
NurbCurve3d nurbsCurve = /* existing NURBS curve */;

// Sample the curve to create polyline approximation
int numSegments = 50;
Point3dCollection polylinePoints = new Point3dCollection();

for (int i = 0; i <= numSegments; i++)
{
    double param = i / (double)numSegments;
    Point3d pt = nurbsCurve.EvaluatePoint(param);
    polylinePoints.Add(pt);
}

// Create polyline entity
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = new Polyline();
    pline.SetDatabaseDefaults();
    
    for (int i = 0; i < polylinePoints.Count; i++)
    {
        Point3d pt = polylinePoints[i];
        pline.AddVertexAt(i, new Point2d(pt.X, pt.Y), 0, 0, 0);
    }
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}

ed.WriteMessage($"\nNURBS converted to polyline with {polylinePoints.Count} vertices");
```

### Example 6: Finding Closest Point on NURBS
```csharp
NurbCurve3d curve = /* existing NURBS curve */;
Point3d testPoint = new Point3d(15, 7, 0);

// Find closest point on curve
Point3d closestPoint = curve.GetClosestPointTo(testPoint);
double distance = curve.GetDistanceTo(testPoint);

ed.WriteMessage($"\nTest point: {testPoint}");
ed.WriteMessage($"\nClosest point on curve: {closestPoint}");
ed.WriteMessage($"\nDistance: {distance:F4}");

// Draw line showing closest point
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = new Line(testPoint, closestPoint);
    line.ColorIndex = 1; // Red
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    tr.Commit();
}
```

## Common Patterns

### Creating Uniform Knot Vector
```csharp
// For degree d and n control points: knots = n + d + 1
int degree = 3;
int numControlPoints = 6;
int numKnots = numControlPoints + degree + 1; // 10 knots

DoubleCollection knots = new DoubleCollection();

// Clamped knot vector (curve passes through first and last control points)
for (int i = 0; i <= degree; i++)
    knots.Add(0.0); // d+1 zeros at start

for (int i = 1; i < numControlPoints - degree; i++)
    knots.Add(i / (double)(numControlPoints - degree));

for (int i = 0; i <= degree; i++)
    knots.Add(1.0); // d+1 ones at end
```

### Checking NURBS Properties
```csharp
NurbCurve3d curve = /* existing curve */;

if (curve.IsRational)
    ed.WriteMessage("\nCurve is rational (has weights)");
else
    ed.WriteMessage("\nCurve is non-rational");

if (curve.IsPeriodic)
    ed.WriteMessage("\nCurve is periodic (closed)");
else
    ed.WriteMessage("\nCurve is open");

ed.WriteMessage($"\nDegree: {curve.Degree}");
ed.WriteMessage($"\nControl Points: {curve.NumControlPoints}");
```

## Best Practices

1. **Knot Vector**: Ensure knot vector length = numControlPoints + degree + 1
2. **Clamped Curves**: Use repeated knots at start/end for curves through endpoints
3. **Weights**: Use weights > 1 to pull curve toward control point, < 1 to push away
4. **Degree**: Higher degree = smoother curve, but more computation
5. **Periodic Curves**: For closed curves, use periodic NURBS with matching start/end
6. **Evaluation**: Sample curve with sufficient points for accurate polyline conversion
7. **Performance**: Cache evaluated points if evaluating same parameters repeatedly

## Related Classes
- **NurbCurve2d** - 2D NURBS curve
- **CubicSplineCurve3d** - Cubic interpolation spline
- **Polyline3d** - Piecewise linear approximation
- **Point3d** - Control points and evaluated points
- **Vector3d** - Tangent vectors and derivatives

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [NurbCurve3d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_NurbCurve3d)
- [NURBS on Wikipedia](https://en.wikipedia.org/wiki/Non-uniform_rational_B-spline)
