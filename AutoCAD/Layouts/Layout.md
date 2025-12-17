# Layout

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `Layout` class represents a layout (paper space or model space) in an AutoCAD drawing. Layouts define the paper size, plot settings, and viewports for organizing and plotting drawings.

**Key Concept:** Every drawing has at least two layouts: Model space and one or more paper space layouts. Each layout can have its own plot settings and viewports.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ PlotSettings
              └─ Layout
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `LayoutName` | `string` | Gets/sets the layout name |
| `TabOrder` | `int` | Gets/sets the tab display order |
| `TabSelected` | `bool` | Gets/sets whether layout tab is selected |
| `BlockTableRecordId` | `ObjectId` | Gets the associated block table record |
| `ModelType` | `bool` | Checks if this is the Model layout |

## Common Usage Patterns

### 1. Listing All Layouts

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("LISTLAYOUTS")]
public void ListAllLayouts()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        ed.WriteMessage("\n=== Layouts ===");
        
        foreach (DBDictionaryEntry entry in layoutDict)
        {
            Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
            
            ed.WriteMessage($"\n{layout.LayoutName}");
            ed.WriteMessage($"\n  Tab Order: {layout.TabOrder}");
            ed.WriteMessage($"\n  Model Type: {layout.ModelType}");
            ed.WriteMessage($"\n  Selected: {layout.TabSelected}");
        }
        
        tr.Commit();
    }
}
```

### 2. Creating New Layout

```csharp
[CommandMethod("CREATELAYOUT")]
public void CreateNewLayout()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for layout name
    PromptStringOptions pso = new PromptStringOptions("\nEnter layout name: ");
    pso.AllowSpaces = true;
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string layoutName = pr.StringResult;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
            OpenMode.ForWrite) as DBDictionary;
        
        // Check if layout already exists
        if (layoutDict.Contains(layoutName))
        {
            ed.WriteMessage($"\nLayout '{layoutName}' already exists");
            tr.Commit();
            return;
        }
        
        // Create new layout
        Layout layout = new Layout();
        layout.LayoutName = layoutName;
        layout.AddToLayoutDictionary(db, layoutDict.ObjectId);
        
        tr.AddNewlyCreatedDBObject(layout, true);
        
        ed.WriteMessage($"\nLayout '{layoutName}' created");
        tr.Commit();
    }
}
```

### 3. Switching to Layout

```csharp
[CommandMethod("SWITCHLAYOUT")]
public void SwitchToLayout()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for layout name
    PromptStringOptions pso = new PromptStringOptions("\nEnter layout name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        if (!layoutDict.Contains(pr.StringResult))
        {
            ed.WriteMessage($"\nLayout '{pr.StringResult}' not found");
            tr.Commit();
            return;
        }
        
        Layout layout = tr.GetObject(layoutDict.GetAt(pr.StringResult), 
            OpenMode.ForWrite) as Layout;
        
        LayoutManager.Current.CurrentLayout = layout.LayoutName;
        
        ed.WriteMessage($"\nSwitched to layout '{layout.LayoutName}'");
        tr.Commit();
    }
}
```

### 4. Deleting Layout

```csharp
[CommandMethod("DELETELAYOUT")]
public void DeleteLayout()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for layout name
    PromptStringOptions pso = new PromptStringOptions("\nEnter layout name to delete: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string layoutName = pr.StringResult;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
            OpenMode.ForWrite) as DBDictionary;
        
        if (!layoutDict.Contains(layoutName))
        {
            ed.WriteMessage($"\nLayout '{layoutName}' not found");
            tr.Commit();
            return;
        }
        
        Layout layout = tr.GetObject(layoutDict.GetAt(layoutName), 
            OpenMode.ForRead) as Layout;
        
        if (layout.ModelType)
        {
            ed.WriteMessage("\nCannot delete Model layout");
            tr.Commit();
            return;
        }
        
        // Use LayoutManager to delete
        LayoutManager.Current.DeleteLayout(layoutName);
        
        ed.WriteMessage($"\nLayout '{layoutName}' deleted");
        tr.Commit();
    }
}
```

### 5. Copying Layout

```csharp
[CommandMethod("COPYLAYOUT")]
public void CopyLayout()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for source layout
    PromptStringOptions pso = new PromptStringOptions("\nEnter source layout name: ");
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string sourceName = pr.StringResult;
    
    // Prompt for new layout name
    pso = new PromptStringOptions("\nEnter new layout name: ");
    pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    string newName = pr.StringResult;
    
    // Use LayoutManager to copy
    LayoutManager.Current.CopyLayout(sourceName, newName);
    
    ed.WriteMessage($"\nLayout '{sourceName}' copied to '{newName}'");
}
```

### 6. Setting Layout Plot Settings

```csharp
[CommandMethod("SETLAYOUTPLOT")]
public void SetLayoutPlotSettings()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Layout layout = tr.GetObject(LayoutManager.Current.GetLayoutId(
            LayoutManager.Current.CurrentLayout), OpenMode.ForWrite) as Layout;
        
        // Set plot settings
        layout.SetPlotSettingsName("DWG To PDF.pc3");
        layout.SetCanonicalMediaName("ISO_A4_(210.00_x_297.00_MM)");
        layout.SetPlotPaperUnits(PlotPaperUnit.Millimeters);
        layout.SetPlotType(PlotType.Layout);
        layout.SetPlotRotation(PlotRotation.Degrees000);
        layout.SetPlotCentered(true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nPlot settings updated");
    }
}
```

## Best Practices

1. **Don't Delete Model**: Never delete the Model layout
2. **Unique Names**: Ensure layout names are unique
3. **Use LayoutManager**: Prefer LayoutManager methods for operations
4. **Tab Order**: Set logical tab order for user navigation
5. **Plot Settings**: Configure plot settings per layout
6. **Viewport Management**: Manage viewports through layout's block table record

## Related Classes

- [PlotSettings](PlotSettings.md) - Base class for plot configuration
- [LayoutManager](LayoutManager.md) - Manages layout operations
- [Viewport](../Entities/Complex/Viewport.md) - Layout viewport entity
- [BlockTableRecord](../SymbolTables/BlockTable.md) - Contains layout entities

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-Layout)
