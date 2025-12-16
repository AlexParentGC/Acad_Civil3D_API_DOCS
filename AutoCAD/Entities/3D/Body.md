# Body Class

## Overview
`Body` is a generic wrapper for an ACIS body that doesn't fit strictly into `Solid3d` (solid volume) or `Region` (planar area) categories. It is often used for:
- Sheet bodies (surfaces).
- Wire bodies (3D wireframes).
- Bodies resulting from failed boolean operations or indeterminate states.
- Converting between other 3D formats.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Body
```

## Key Properties
*Inherits most properties from Entity.*

| Property | Type | Description |
|----------|------|-------------|
| `IsNull` | `bool` | True if the internal ACIS pointer is null. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SetBody(IntPtr)` | `void` | Sets the internal ACIS body pointer (Advanced). |
| `GetBody()` | `IntPtr` | Gets the internal ACIS body pointer. |

## Code Examples

### Example 1: Creating a Body check
```csharp
// Bodies are rarely created directly via "new Body()".
// They usually result from modeling operations or import.
Body body = ent as Body;
if (body != null)
{
    // It's a generic ACIS body
}
```

### Example 2: Converting Solid to Body
```csharp
// Sometimes detailed analysis requires treating a solid as a generic body
// This is more common in ObjectARX (C++) than .NET
```

### Example 3: Using Body for BREP (Boundary Rep)
```csharp
using Autodesk.AutoCAD.BoundaryRepresentation;

// Use the BREP API to analyze topology
using (Brep brep = new Brep(someEntity))
{
    foreach (Autodesk.AutoCAD.BoundaryRepresentation.Face face in brep.Faces)
    {
        // Analyze faces of the body/solid/region
    }
}
```

### Example 4: Identifying ACIS Type
```csharp
// Since Body is generic, it's hard to know what it is (Sheet? Wire?)
// Often checking Mass Properties or using Brep API is needed.
```

### Example 5: Exploding complex imports
```csharp
// Imported STEP/IGES files often come in as Bodies
// Exploding them might yield Surfaces or Curves
DBObjectCollection parts = new DBObjectCollection();
body.Explode(parts);
```

### Example 6: Validity Check
```csharp
if (!body.IsNull) 
{ 
    // valid 
}
```

### Example 7: ACIS Modeling Integration
```csharp
// Body is often used as a bridge to external kernels or legacy SAT files
```

### Example 8: Handling "Sheet" Bodies
```csharp
// A "Sheet Body" (surface) in ACIS terms is often wrapped as a Body entity in AutoCAD
// rather than a Surface entity (which is newer).
```

## Best Practices
1. **Prefer Specific Types**: Use `Solid3d` or `Surface` classes if possible. `Body` is a catch-all fallback.
2. **Brep API**: To do anything useful with a `Body` (measure, analyze), you usually need the `Autodesk.AutoCAD.BoundaryRepresentation` namespace.
3. **Performance**: Like all ACIS wrappers, they are heavy. Dispose when done.

## Related Objects
- [Solid3d](Solid3d.md)
- [Region](Region.md)

## References
- [Autodesk Body Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Body)
