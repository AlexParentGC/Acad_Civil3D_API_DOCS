# Parcel Class

## Overview
`Parcel` represents a 2D closed area in Civil 3D, typically representing a plot of land or a right-of-way. Parcels are dynamic topological entities; they are automatically formed and updated by the geometry (Parcel Segments or Alignments) within their parent `Site`.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Parcel
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Area` | `double` | Area of the parcel. |
| `Perimeter` | `double` | Total perimeter. |
| `Name` | `string` | Parcel name/number. |
| `Number` | `int` | Automatic parcel number. |
| `SiteId` | `ObjectId` | The site it belongs to. |
| `UserDefinedProperties` | `UDP` | Custom property bucket. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Create(...)` | `ObjectId` | Static methods to create from objects. |
| `CreateFromObjects(...)` | `ObjectId` | Creates parcels from CAD polylines. |

## Code Examples

### Example 1: Creating Parcels from Polylines
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectId siteId = ...; // Get target Site
    ObjectIdCollection polyIds = ...; // Polylines to convert
    
    ObjectIdCollection createdParcels = Parcel.CreateFromObjects(
        polyIds, 
        siteId, 
        db.CurrentSpaceId, 
        false // Erase existing entities?
    );
    
    ed.WriteMessage($"\nCreated {createdParcels.Count} parcels.");
    tr.Commit();
}
```

### Example 2: Reading Parcel Area
```csharp
Parcel p = tr.GetObject(id, OpenMode.ForRead) as Parcel;
ed.WriteMessage($"\nParcel {p.Name}: Area = {p.Area:F2} sq ft");
```

### Example 3: Renumbering Parcels
```csharp
// Renumbering is usually a Site-level operation or done via properties
p.Name = "Lot " + p.Number;
```

### Example 4: Getting Segment Geometry
```csharp
// Parcels don't own the geometry directly; they are defined BY segments.
// To get the shape, you might look at the base curves.
```

### Example 5: Working with UDP (User Defined Properties)
```csharp
// Civil 3D allows custom fields on Parcels (e.g., "Zoning", "Owner")
// Accessing these requires the UDP API (UserDefinedPropertyClassification)
// This is advanced and involves `Parcel.GetUserDefinedPropertyValue`.
```

### Example 6: Topological Updates
```csharp
// If you draw an Alignment through this Parcel (in the same Site),
// The Parcel will split into two AUTOMATICALLY.
// The API reflects this: the original Parcel object usually shrinks,
// and a new Parcel object is created for the split piece.
```

### Example 7: Inverse/Map Check
```csharp
// Getting the "Legal Description" geometry
// Iterate segments...
```

### Example 8: Styles
```csharp
p.StyleId = newStyleId; // Change visual style (e.g., "Single-Family")
p.AreaLabelStyleId = newLabelStyleId; // Change label style
```

## Best Practices
1. **Topology Awareness**: Remember that modifying ONE segment can affect TWO parcels (shared edge).
2. **Site Management**: Ensure you are working in the correct Site. Parcels cannot exist without a Site.
3. **Automatic Updates**: Because Parcels react to other geometry, GUIDs/IDs are stable, but geometry is volatile.

## Related Objects
- [Site](../Core/Site.md) - The container.
- [Alignment](../Alignment/Alignment.md) - Can form parcel boundaries.

## References
- [Autodesk Parcel Reference](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=...)
