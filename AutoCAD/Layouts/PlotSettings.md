# PlotSettings

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `PlotSettings` class defines plot configuration settings including plotter, paper size, plot area, scale, and other plotting parameters. It serves as the base class for the Layout class.

**Key Concept:** PlotSettings can be saved as named plot settings or embedded in layouts. They define how a drawing will be plotted.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ PlotSettings
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `PlotSettingsName` | `string` | Gets/sets the plot settings name |
| `PlotConfigurationName` | `string` | Gets/sets the plotter/printer name |
| `CanonicalMediaName` | `string` | Gets/sets the paper size |
| `PlotPaperUnits` | `PlotPaperUnit` | Gets/sets units (inches/mm) |
| `PlotRotation` | `PlotRotation` | Gets/sets plot rotation |
| `PlotCentered` | `bool` | Gets/sets whether plot is centered |
| `PlotType` | `PlotType` | Gets/sets what to plot (extents/window/layout) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `CopyFrom(PlotSettings)` | `void` | Copies settings from another PlotSettings |
| `RefreshLists(bool)` | `void` | Refreshes available plotters and paper sizes |

## Common Usage Patterns

### 1. Creating Named Plot Settings

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.PlottingServices;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("CREATEPLOTSETTINGS")]
public void CreatePlotSettings()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary plotSettingsDict = tr.GetObject(db.PlotSettingsDictionaryId, 
            OpenMode.ForWrite) as DBDictionary;
        
        PlotSettings ps = new PlotSettings(true); // true = model type
        ps.PlotSettingsName = "MyPlotSettings";
        ps.AddToPlotSettingsDictionary(db);
        tr.AddNewlyCreatedDBObject(ps, true);
        
        // Configure settings
        PlotSettingsValidator psv = PlotSettingsValidator.Current;
        psv.SetPlotConfigurationName(ps, "DWG To PDF.pc3", null);
        psv.SetCanonicalMediaName(ps, "ISO_A4_(210.00_x_297.00_MM)");
        psv.SetPlotPaperUnits(ps, PlotPaperUnit.Millimeters);
        psv.SetPlotRotation(ps, PlotRotation.Degrees000);
        psv.SetPlotType(ps, PlotType.Extents);
        psv.SetUseStandardScale(ps, true);
        psv.SetStdScaleType(ps, StdScaleType.ScaleToFit);
        psv.SetPlotCentered(ps, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nPlot settings created");
    }
}
```

### 2. Applying Plot Settings to Layout

```csharp
[CommandMethod("APPLYPLOTSETTINGS")]
public void ApplyPlotSettingsToLayout()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Layout layout = tr.GetObject(LayoutManager.Current.GetLayoutId(
            LayoutManager.Current.CurrentLayout), OpenMode.ForWrite) as Layout;
        
        PlotSettingsValidator psv = PlotSettingsValidator.Current;
        
        // Set plotter
        psv.SetPlotConfigurationName(layout, "DWG To PDF.pc3", "ANSI_A_(8.50_x_11.00_Inches)");
        
        // Set plot area
        psv.SetPlotType(layout, PlotType.Layout);
        
        // Set scale
        psv.SetUseStandardScale(layout, true);
        psv.SetStdScaleType(layout, StdScaleType.StdScale1To1);
        
        // Center plot
        psv.SetPlotCentered(layout, true);
        
        // Set rotation
        psv.SetPlotRotation(layout, PlotRotation.Degrees000);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nPlot settings applied to layout");
    }
}
```

### 3. Listing Available Plotters

```csharp
[CommandMethod("LISTPLOTTERS")]
public void ListAvailablePlotters()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        PlotSettings ps = new PlotSettings(true);
        ps.AddToPlotSettingsDictionary(db);
        tr.AddNewlyCreatedDBObject(ps, true);
        
        PlotSettingsValidator psv = PlotSettingsValidator.Current;
        psv.RefreshLists(ps);
        
        StringCollection plotters = psv.GetPlotDeviceList();
        
        ed.WriteMessage("\n=== Available Plotters ===");
        foreach (string plotter in plotters)
        {
            ed.WriteMessage($"\n{plotter}");
        }
        
        ps.Erase();
        tr.Commit();
    }
}
```

### 4. Listing Paper Sizes for Plotter

```csharp
[CommandMethod("LISTPAPERSIZES")]
public void ListPaperSizes()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for plotter name
    PromptStringOptions pso = new PromptStringOptions("\nEnter plotter name: ");
    pso.DefaultValue = "DWG To PDF.pc3";
    PromptResult pr = ed.GetString(pso);
    if (pr.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        PlotSettings ps = new PlotSettings(true);
        ps.AddToPlotSettingsDictionary(db);
        tr.AddNewlyCreatedDBObject(ps, true);
        
        PlotSettingsValidator psv = PlotSettingsValidator.Current;
        psv.SetPlotConfigurationName(ps, pr.StringResult, null);
        psv.RefreshLists(ps);
        
        StringCollection paperSizes = psv.GetCanonicalMediaNameList(ps);
        
        ed.WriteMessage($"\n=== Paper Sizes for {pr.StringResult} ===");
        foreach (string size in paperSizes)
        {
            ed.WriteMessage($"\n{size}");
        }
        
        ps.Erase();
        tr.Commit();
    }
}
```

### 5. Batch Plot Multiple Layouts

```csharp
[CommandMethod("BATCHPLOT")]
public void BatchPlotLayouts()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
            OpenMode.ForRead) as DBDictionary;
        
        PlotEngine pe = PlotFactory.CreatePublishEngine();
        
        foreach (DBDictionaryEntry entry in layoutDict)
        {
            Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
            
            if (!layout.ModelType) // Skip Model space
            {
                PlotInfo pi = new PlotInfo();
                pi.Layout = layout.ObjectId;
                
                PlotInfoValidator piv = new PlotInfoValidator();
                piv.MediaMatchingPolicy = MatchingPolicy.MatchEnabled;
                piv.Validate(pi);
                
                ed.WriteMessage($"\nPlotting layout: {layout.LayoutName}");
                
                // Plot would be executed here
                // pe.BeginPlot(null, null);
                // pe.BeginDocument(pi, doc.Name, null, 1, true, "C:\\Temp\\");
                // pe.BeginPage(...);
                // pe.EndPage();
                // pe.EndDocument();
                // pe.EndPlot();
            }
        }
        
        tr.Commit();
    }
}
```

## Best Practices

1. **Use PlotSettingsValidator**: Always use PlotSettingsValidator to modify settings
2. **Refresh Lists**: Call RefreshLists() before querying available devices
3. **Validate Settings**: Ensure plotter and paper size combinations are valid
4. **Named Settings**: Create reusable named plot settings
5. **Test First**: Test plot settings before batch operations
6. **Error Handling**: Wrap plot operations in try-catch blocks

## Related Classes

- [Layout](Layout.md) - Inherits from PlotSettings
- [PlotSettingsValidator](PlotSettingsValidator.md) - Validates and modifies plot settings
- [PlotInfo](PlotInfo.md) - Plot execution information
- [PlotEngine](PlotEngine.md) - Executes plotting operations

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-PlotSettings)
