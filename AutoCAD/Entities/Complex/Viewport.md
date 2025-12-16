# Viewport Class

## Overview
The `Viewport` class represents a layout viewport entity in AutoCAD. Viewports are windows in paper space layouts that display views of model space geometry. They allow you to create multiple views of your model at different scales, orientations, and layer visibility settings on a single layout sheet.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Viewport
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ViewCenter` | `Point2d` | Gets/sets the center point of the view |
| `ViewTarget` | `Point3d` | Gets/sets the view target point in model space |
| `ViewDirection` | `Vector3d` | Gets/sets the viewing direction |
| `ViewHeight` | `double` | Gets/sets the view height in model space units |
| `CustomScale` | `double` | Gets/sets the custom viewport scale |
| `StandardScale` | `StandardScaleType` | Gets/sets the standard scale type |
| `Width` | `double` | Gets/sets the viewport width |
| `Height` | `double` | Gets/sets the viewport height |
| `CenterPoint` | `Point3d` | Gets/sets the viewport center in paper space |
| `On` | `bool` | Gets/sets whether the viewport is active |
| `Locked` | `bool` | Gets/sets whether the viewport is locked |
| `NonRectClipEntityId` | `ObjectId` | Gets/sets the non-rectangular clipping boundary |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SyncModelView()` | `void` | Synchronizes the viewport with model space view |
| `GetFrozenLayerList()` | `ObjectIdCollection` | Gets layers frozen in this viewport |
| `FreezeLayersInViewport(ObjectIdCollection)` | `void` | Freezes layers in this viewport |
| `ThawLayersInViewport(ObjectIdCollection)` | `void` | Thaws layers in this viewport |
| `IsLayerFrozenInViewport(ObjectId)` | `bool` | Checks if a layer is frozen in viewport |

## Code Examples

### Example 1: Creating a Simple Viewport
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get a layout (not Model)
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    Layout layout = tr.GetObject(layoutDict.GetAt("Layout1"), OpenMode.ForRead) as Layout;
    
    BlockTableRecord layoutBtr = tr.GetObject(layout.BlockTableRecordId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create viewport
    Viewport viewport = new Viewport();
    viewport.SetDatabaseDefaults();
    
    // Set position and size in paper space
    viewport.CenterPoint = new Point3d(150, 100, 0); // Paper space units (mm)
    viewport.Width = 200;
    viewport.Height = 150;
    
    // Set view properties
    viewport.ViewCenter = new Point2d(0, 0); // Model space center
    viewport.ViewHeight = 100; // Model space height
    
    // Turn on the viewport
    viewport.On = true;
    
    // Add to layout
    layoutBtr.AppendEntity(viewport);
    tr.AddNewlyCreatedDBObject(viewport, true);
    
    tr.Commit();
}
```

### Example 2: Setting Viewport Scale
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    
    // Set standard scale (1:50)
    viewport.StandardScale = StandardScaleType.Scale1To50;
    
    // Or set custom scale
    // viewport.CustomScale = 50.0; // 1:50 scale
    
    ed.WriteMessage($"\nViewport scale set to 1:50");
    ed.WriteMessage($"\nView height: {viewport.ViewHeight:F2}");
    
    tr.Commit();
}
```

### Example 3: Freezing Layers in Viewport
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    // Get layers to freeze
    ObjectIdCollection layersToFreeze = new ObjectIdCollection();
    
    if (lt.Has("Dimensions"))
        layersToFreeze.Add(lt["Dimensions"]);
    if (lt.Has("Text"))
        layersToFreeze.Add(lt["Text"]);
    
    // Freeze layers in this viewport only
    viewport.FreezeLayersInViewport(layersToFreeze);
    
    ed.WriteMessage($"\nFroze {layersToFreeze.Count} layers in viewport");
    
    tr.Commit();
}
```

### Example 4: Reading Viewport Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForRead) as Viewport;
    
    ed.WriteMessage("\n=== Viewport Information ===");
    ed.WriteMessage($"\nOn: {viewport.On}");
    ed.WriteMessage($"\nLocked: {viewport.Locked}");
    ed.WriteMessage($"\nCenter Point: {viewport.CenterPoint}");
    ed.WriteMessage($"\nWidth: {viewport.Width:F2}");
    ed.WriteMessage($"\nHeight: {viewport.Height:F2}");
    ed.WriteMessage($"\nView Center: {viewport.ViewCenter}");
    ed.WriteMessage($"\nView Height: {viewport.ViewHeight:F2}");
    ed.WriteMessage($"\nView Target: {viewport.ViewTarget}");
    ed.WriteMessage($"\nView Direction: {viewport.ViewDirection}");
    ed.WriteMessage($"\nCustom Scale: {viewport.CustomScale:F2}");
    ed.WriteMessage($"\nStandard Scale: {viewport.StandardScale}");
    
    // List frozen layers
    ObjectIdCollection frozenLayers = viewport.GetFrozenLayerList();
    ed.WriteMessage($"\n\nFrozen Layers: {frozenLayers.Count}");
    
    foreach (ObjectId layerId in frozenLayers)
    {
        LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
        ed.WriteMessage($"\n  - {ltr.Name}");
    }
    
    tr.Commit();
}
```

### Example 5: Locking/Unlocking Viewport
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    
    // Lock viewport to prevent accidental pan/zoom
    viewport.Locked = true;
    
    ed.WriteMessage($"\nViewport locked: {viewport.Locked}");
    
    tr.Commit();
}
```

### Example 6: Creating Multiple Viewports on Layout
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    Layout layout = tr.GetObject(layoutDict.GetAt("Layout1"), OpenMode.ForRead) as Layout;
    BlockTableRecord layoutBtr = tr.GetObject(layout.BlockTableRecordId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create 4 viewports in a grid
    double[] scales = { 1.0, 2.0, 4.0, 8.0 };
    string[] scaleNames = { "1:1", "1:2", "1:4", "1:8" };
    
    for (int i = 0; i < 4; i++)
    {
        Viewport vp = new Viewport();
        vp.SetDatabaseDefaults();
        
        // Position in 2x2 grid
        int row = i / 2;
        int col = i % 2;
        vp.CenterPoint = new Point3d(100 + col * 150, 200 - row * 120, 0);
        vp.Width = 120;
        vp.Height = 90;
        
        // Set view
        vp.ViewCenter = new Point2d(0, 0);
        vp.CustomScale = scales[i];
        vp.On = true;
        
        layoutBtr.AppendEntity(vp);
        tr.AddNewlyCreatedDBObject(vp, true);
        
        ed.WriteMessage($"\nCreated viewport {i + 1} at scale {scaleNames[i]}");
    }
    
    tr.Commit();
}
```

### Example 7: Setting Viewport View Direction
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    
    // Set isometric view (SW Isometric)
    viewport.ViewDirection = new Vector3d(-1, -1, 1).GetNormal();
    viewport.ViewTarget = Point3d.Origin;
    
    ed.WriteMessage("\nSet viewport to SW Isometric view");
    
    tr.Commit();
}
```

### Example 8: Thawing All Layers in Viewport
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    
    // Get all frozen layers
    ObjectIdCollection frozenLayers = viewport.GetFrozenLayerList();
    
    if (frozenLayers.Count > 0)
    {
        // Thaw all layers
        viewport.ThawLayersInViewport(frozenLayers);
        
        ed.WriteMessage($"\nThawed {frozenLayers.Count} layers in viewport");
    }
    else
    {
        ed.WriteMessage("\nNo frozen layers in viewport");
    }
    
    tr.Commit();
}
```

## Standard Scale Types

Common standard scales include:
- `Scale1To1` - 1:1 (full size)
- `Scale1To2` - 1:2
- `Scale1To4` - 1:4
- `Scale1To5` - 1:5
- `Scale1To8` - 1:8
- `Scale1To10` - 1:10
- `Scale1To16` - 1:16
- `Scale1To20` - 1:20
- `Scale1To50` - 1:50
- `Scale1To100` - 1:100

## Viewport States

| Property | Description |
|----------|-------------|
| `On = true` | Viewport displays model space content |
| `On = false` | Viewport is blank (off) |
| `Locked = true` | Pan/zoom locked, prevents accidental changes |
| `Locked = false` | Pan/zoom enabled |

## Best Practices

1. **Lock Viewports**: Lock viewports after setting up to prevent accidental changes
2. **Layer Freezing**: Use viewport-specific layer freezing for different views
3. **Standard Scales**: Use standard scales for consistency
4. **Viewport Arrangement**: Organize viewports logically on layouts
5. **Turn Off When Not Needed**: Turn off viewports to improve performance
6. **Named Views**: Save named views for complex viewport setups
7. **Visual Styles**: Set different visual styles per viewport
8. **Clipping**: Use non-rectangular clipping for custom viewport shapes

## Common Use Cases

- **Multi-Scale Drawings**: Show overall and detail views
- **Plan/Elevation/Section**: Multiple views of same model
- **Different Layer Visibility**: Show/hide layers per viewport
- **Isometric Views**: 3D isometric alongside plan views
- **Detail Callouts**: Enlarged detail views
- **Comparison Views**: Before/after or design options

## Paper Space vs Model Space

| Aspect | Paper Space | Model Space |
|--------|-------------|-------------|
| Units | Typically mm or inches | Real-world units |
| Purpose | Layout and printing | Design and modeling |
| Viewports | Container for viewports | Viewed through viewports |
| Scale | 1:1 (paper size) | Varies per viewport |

## Related Objects
- [Entity](../../BaseClasses/Entity.md) - Base class for Viewport
- Layout - Paper space layout containing viewports
- [Database](../../Core/Database.md) - Contains layout dictionary
- LayerTableRecord - Layers that can be frozen per viewport

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Viewport Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Viewport)
- [Working with Layouts and Viewports](https://help.autodesk.com/view/OARX/2024/ENU/)
