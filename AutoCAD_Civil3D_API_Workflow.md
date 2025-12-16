# AutoCAD and Civil3D .NET API - Object Acquisition Workflow

This document provides a comprehensive workflow for accessing all object types in the AutoCAD and Civil3D .NET API object model tree using C#.

## Table of Contents
- [AutoCAD API Workflow](#autocad-api-workflow)
- [Civil3D API Workflow](#civil3d-api-workflow)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)

---

## AutoCAD API Workflow

### 1. Accessing the Application

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;

// Get the current AutoCAD application
Application acApp = Application.DocumentManager.MdiActiveDocument.Application;
```

### 2. Accessing Documents

```csharp
// Get the Document Manager
DocumentCollection docMgr = Application.DocumentManager;

// Get the active document
Document acDoc = docMgr.MdiActiveDocument;

// Or iterate through all open documents
foreach (Document doc in docMgr)
{
    // Work with each document
}
```

### 3. Accessing the Database

```csharp
// Get the database from the active document
Database db = acDoc.Database;

// Or get the working database
Database workingDb = HostApplicationServices.WorkingDatabase;
```

### 4. Accessing Symbol Tables

All symbol table access follows a similar pattern using transactions:

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // BlockTable
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // LayerTable
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    // LinetypeTable
    LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
    
    // TextStyleTable
    TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForRead) as TextStyleTable;
    
    // DimStyleTable
    DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
    
    // UcsTable
    UcsTable ut = tr.GetObject(db.UcsTableId, OpenMode.ForRead) as UcsTable;
    
    // ViewTable
    ViewTable vt = tr.GetObject(db.ViewTableId, OpenMode.ForRead) as ViewTable;
    
    // ViewportTable
    ViewportTable vpt = tr.GetObject(db.ViewportTableId, OpenMode.ForRead) as ViewportTable;
    
    // RegAppTable (Registered Applications)
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    tr.Commit();
}
```

### 5. Accessing BlockTableRecords and Entities

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Access Model Space
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Access Paper Space
    BlockTableRecord paperSpace = tr.GetObject(bt[BlockTableRecord.PaperSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Iterate through entities in Model Space
    foreach (ObjectId objId in modelSpace)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        // Check entity type and cast appropriately
        if (ent is Line line)
        {
            // Work with line
        }
        else if (ent is Arc arc)
        {
            // Work with arc
        }
        else if (ent is Circle circle)
        {
            // Work with circle
        }
        else if (ent is Polyline pline)
        {
            // Work with lightweight polyline
        }
        else if (ent is Polyline2d pline2d)
        {
            // Work with 2D polyline
        }
        else if (ent is Polyline3d pline3d)
        {
            // Work with 3D polyline
        }
        else if (ent is Ellipse ellipse)
        {
            // Work with ellipse
        }
        else if (ent is Spline spline)
        {
            // Work with spline
        }
        else if (ent is BlockReference blockRef)
        {
            // Work with block reference
        }
        else if (ent is DBText dbText)
        {
            // Work with single-line text
        }
        else if (ent is MText mText)
        {
            // Work with multi-line text
        }
        else if (ent is Dimension dim)
        {
            // Work with dimension
        }
        else if (ent is Hatch hatch)
        {
            // Work with hatch
        }
        else if (ent is Leader leader)
        {
            // Work with leader
        }
        else if (ent is MLeader mLeader)
        {
            // Work with multileader
        }
        else if (ent is Solid3d solid)
        {
            // Work with 3D solid
        }
        else if (ent is Region region)
        {
            // Work with region
        }
        else if (ent is Body body)
        {
            // Work with body
        }
        else if (ent is SubDMesh mesh)
        {
            // Work with subdivision mesh
        }
        else if (ent is Viewport viewport)
        {
            // Work with viewport
        }
    }
    
    tr.Commit();
}
```

### 6. Working with Curve Objects

All curve-based entities inherit from the `Curve` class:

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    foreach (ObjectId objId in btr)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        if (ent is Curve curve)
        {
            // Common curve properties and methods
            double length = curve.GetDistanceAtParameter(curve.EndParam);
            Point3d startPoint = curve.StartPoint;
            Point3d endPoint = curve.EndPoint;
            
            // Get point at parameter
            Point3d midPoint = curve.GetPointAtParameter(
                (curve.StartParam + curve.EndParam) / 2);
        }
    }
    
    tr.Commit();
}
```

### 7. Accessing UI Components

```csharp
using Autodesk.Windows; // For Ribbon
using Autodesk.AutoCAD.Windows; // For PaletteSet

// Accessing the Ribbon
RibbonControl ribbon = ComponentManager.Ribbon;
if (ribbon != null)
{
    // Work with ribbon tabs and panels
}

// Creating a PaletteSet
PaletteSet ps = new PaletteSet("My Palette");
ps.Add("My Control", new System.Windows.Forms.UserControl());
ps.Visible = true;

// Accessing the Editor
Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
```

### 8. Creating Ribbon Elements

```csharp
using Autodesk.Windows;
using System.Windows.Input;

public void CreateRibbon()
{
    RibbonControl ribbon = ComponentManager.Ribbon;
    if (ribbon == null) return;

    // 1. Create a Tab
    RibbonTab tab = new RibbonTab();
    tab.Title = "My Custom Tab";
    tab.Id = "MY_CUSTOM_TAB_ID";
    ribbon.Tabs.Add(tab);

    // 2. Create a Panel
    RibbonPanelSource panelSource = new RibbonPanelSource();
    panelSource.Title = "My Panel";
    RibbonPanel panel = new RibbonPanel();
    panel.Source = panelSource;
    tab.Panels.Add(panel);

    // 3. Create a Button
    RibbonButton button = new RibbonButton();
    button.Text = "My Button";
    button.ShowText = true;
    button.ShowImage = true;
    // button.Image = ... (Load BitmapImage)
    button.Size = RibbonItemSize.Large;
    button.Orientation = System.Windows.Controls.Orientation.Vertical;

    // 4. Assign Command Handler
    button.CommandHandler = new RibbonCommandHandler();
    button.CommandParameter = "MY_COMMAND "; // Space at end to execute

    panelSource.Items.Add(button);
    
    // Set tab as active (optional)
    tab.IsActive = true;
}

// Command Handler Implementation
public class RibbonCommandHandler : System.Windows.Input.ICommand
{
    public bool CanExecute(object parameter) => true;
    
    public event EventHandler CanExecuteChanged;
    
    public void Execute(object parameter)
    {
        if (parameter is string cmd)
        {
            // Send command to AutoCAD
            Autodesk.AutoCAD.ApplicationServices.Application.DocumentManager.MdiActiveDocument
                .SendStringToExecute(cmd, true, false, true);
        }
    }
}
```

### 9. Working with Database Objects

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Accessing the Named Objects Dictionary
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    // Check for a specific dictionary
    if (nod.Contains("ACAD_GROUP"))
    {
        ObjectId groupId = nod.GetAt("ACAD_GROUP");
        DBDictionary groupDict = tr.GetObject(groupId, OpenMode.ForRead) as DBDictionary;
    }
    
    // Accessing XRecords (e.g., in Extension Dictionary)
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    if (ent.ExtensionDictionary != ObjectId.Null)
    {
        DBDictionary extDict = tr.GetObject(ent.ExtensionDictionary, OpenMode.ForRead) as DBDictionary;
        if (extDict.Contains("MyData"))
        {
            XRecord xRec = tr.GetObject(extDict.GetAt("MyData"), OpenMode.ForRead) as XRecord;
            foreach (TypedValue tv in xRec.Data)
            {
                // Process data
            }
        }
    }
    
    tr.Commit();
}
```

---

## Civil3D API Workflow

### 1. Accessing the CivilApplication

```csharp
using Autodesk.Civil.ApplicationServices;
using Autodesk.Civil.DatabaseServices;

// Get the Civil Application
CivilApplication civilApp = CivilApplication.ActiveDocument.Application;
```

### 2. Accessing the CivilDocument

```csharp
// Get the active Civil document
CivilDocument civilDoc = CivilApplication.ActiveDocument;

// Or from AutoCAD document
Document acDoc = Application.DocumentManager.MdiActiveDocument;
CivilDocument civilDoc = CivilDocument.GetCivilDocument(acDoc.Database);
```

### 3. Accessing Civil3D Object Collections

All Civil3D objects are accessed via `ObjectIdCollection`:

```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    // Alignments
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    foreach (ObjectId alignId in alignmentIds)
    {
        Alignment alignment = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
        // Work with alignment
    }

    // Sites and Parcels
    ObjectIdCollection siteIds = civilDoc.GetSiteIds();
    foreach (ObjectId siteId in siteIds)
    {
        Site site = tr.GetObject(siteId, OpenMode.ForRead) as Site;
        
        // Get Parcels in this Site
        ObjectIdCollection parcelIds = site.GetParcelIds();
        foreach (ObjectId parcelId in parcelIds)
        {
            Parcel parcel = tr.GetObject(parcelId, OpenMode.ForRead) as Parcel;
            // Work with parcel
        }
        
        // Use site.GetAlignmentIds() for site-specific alignments
    }
    
    // Surfaces
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    foreach (ObjectId surfId in surfaceIds)
    {
        Surface surface = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
        
        // Check surface type
        if (surface is TinSurface tinSurf)
        {
            // Work with TIN surface
        }
        else if (surface is GridSurface gridSurf)
        {
            // Work with Grid surface
        }
        else if (surface is TinVolumeSurface tinVolSurf)
        {
            // Work with TIN Volume surface
        }
        else if (surface is GridVolumeSurface gridVolSurf)
        {
            // Work with Grid Volume surface
        }
    }
    
    // CogoPoints
    ObjectIdCollection cogoPointIds = civilDoc.CogoPoints.GetPointIds();
    foreach (ObjectId pointId in cogoPointIds)
    {
        CogoPoint cogoPoint = tr.GetObject(pointId, OpenMode.ForRead) as CogoPoint;
        // Work with COGO point
    }
    
    // Pipe Networks
    ObjectIdCollection networkIds = civilDoc.GetPipeNetworkIds();
    foreach (ObjectId networkId in networkIds)
    {
        Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
        
        // Get pipes in network
        ObjectIdCollection pipeIds = network.GetPipeIds();
        foreach (ObjectId pipeId in pipeIds)
        {
            Pipe pipe = tr.GetObject(pipeId, OpenMode.ForRead) as Pipe;
            // Work with pipe
        }
        
        // Get structures in network
        ObjectIdCollection structureIds = network.GetStructureIds();
        foreach (ObjectId structId in structureIds)
        {
            Structure structure = tr.GetObject(structId, OpenMode.ForRead) as Structure;
            // Work with structure
        }
    }
    
    // Corridors
    ObjectIdCollection corridorIds = civilDoc.GetCorridorIds();
    foreach (ObjectId corridorId in corridorIds)
    {
        Corridor corridor = tr.GetObject(corridorId, OpenMode.ForRead) as Corridor;
        // Work with corridor
    }
    
    // Assemblies
    ObjectIdCollection assemblyIds = civilDoc.GetAssemblyIds();
    foreach (ObjectId assemblyId in assemblyIds)
    {
        Assembly assembly = tr.GetObject(assemblyId, OpenMode.ForRead) as Assembly;
        // Work with assembly
    }
    
    // Catchments
    ObjectIdCollection catchmentIds = civilDoc.GetCatchmentIds();
    foreach (ObjectId catchmentId in catchmentIds)
    {
        Catchment catchment = tr.GetObject(catchmentId, OpenMode.ForRead) as Catchment;
        // Work with catchment
    }
    
    // Gradings
    ObjectIdCollection gradingIds = civilDoc.GetGradingIds();
    foreach (ObjectId gradingId in gradingIds)
    {
        Grading grading = tr.GetObject(gradingId, OpenMode.ForRead) as Grading;
        // Work with grading
    }
    
    // Feature Lines
    ObjectIdCollection featureLineIds = civilDoc.GetFeatureLineIds();
    foreach (ObjectId featureLineId in featureLineIds)
    {
        FeatureLine featureLine = tr.GetObject(featureLineId, OpenMode.ForRead) as FeatureLine;
        // Work with feature line
    }
    
    // Sample Lines
    ObjectIdCollection sampleLineIds = civilDoc.GetSampleLineGroupIds();
    foreach (ObjectId sampleLineGroupId in sampleLineIds)
    {
        SampleLineGroup sampleLineGroup = tr.GetObject(sampleLineGroupId, 
            OpenMode.ForRead) as SampleLineGroup;
        
        ObjectIdCollection sampleLineIds2 = sampleLineGroup.GetSampleLineIds();
        foreach (ObjectId sampleLineId in sampleLineIds2)
        {
            SampleLine sampleLine = tr.GetObject(sampleLineId, OpenMode.ForRead) as SampleLine;
            // Work with sample line
        }
    }
    
    tr.Commit();
}
```

### 4. Accessing Profiles (Associated with Alignments)

```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    
    foreach (ObjectId alignId in alignmentIds)
    {
        Alignment alignment = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
        
        // Get profile views
        ObjectIdCollection profileViewIds = alignment.GetProfileViewIds();
        
        foreach (ObjectId pvId in profileViewIds)
        {
            ProfileView profileView = tr.GetObject(pvId, OpenMode.ForRead) as ProfileView;
            
            // Get profiles in this view
            ObjectIdCollection profileIds = profileView.GetProfileIds();
            
            foreach (ObjectId profId in profileIds)
            {
                Profile profile = tr.GetObject(profId, OpenMode.ForRead) as Profile;
                // Work with profile
            }
        }
    }
    
    tr.Commit();
}
```

### 5. Accessing Styles and Settings

```csharp
// Access Civil3D Styles
CivilDocument civilDoc = CivilApplication.ActiveDocument;

// Alignment styles
ObjectIdCollection alignmentStyleIds = civilDoc.Styles.AlignmentStyles;

// Surface styles
ObjectIdCollection surfaceStyleIds = civilDoc.Styles.SurfaceStyles;

// Profile styles
ObjectIdCollection profileStyleIds = civilDoc.Styles.ProfileStyles;

// Pipe network styles
ObjectIdCollection pipeStyleIds = civilDoc.Styles.PipeStyles;
ObjectIdCollection structureStyleIds = civilDoc.Styles.StructureStyles;

// Access Civil3D Settings
SettingsAlignment alignmentSettings = civilDoc.Settings.AlignmentSettings;
SettingsSurface surfaceSettings = civilDoc.Settings.SurfaceSettings;
```

---

## Common Patterns

### Transaction Pattern (CRITICAL)

**Always use transactions** when accessing database objects:

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    try
    {
        // Get and work with objects
        
        tr.Commit(); // Commit changes
    }
    catch (System.Exception ex)
    {
        // Handle exception
        tr.Abort(); // Rollback on error
    }
}
```

### Document Lock Pattern

For commands that modify the drawing, lock the document:

```csharp
Document acDoc = Application.DocumentManager.MdiActiveDocument;
Database db = acDoc.Database;

using (DocumentLock docLock = acDoc.LockDocument())
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Modify objects here
        
        tr.Commit();
    }
}
```

### Editor Access Pattern

For user interaction:

```csharp
Document acDoc = Application.DocumentManager.MdiActiveDocument;
Editor ed = acDoc.Editor;

// Prompt for selection
PromptSelectionResult selResult = ed.GetSelection();
if (selResult.Status == PromptStatus.OK)
{
    SelectionSet selSet = selResult.Value;
    // Work with selection
}

// Write messages
ed.WriteMessage("\nMessage to command line");
```

---

## Best Practices

### 1. Always Use Transactions
Never access database objects without a transaction.

### 2. Use `using` Statements
Ensure proper disposal of transactions and other disposable objects.

### 3. Check for Null
Always check if objects exist before using them:

```csharp
Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
if (ent != null)
{
    // Work with entity
}
```

### 4. Use Appropriate OpenMode
- `OpenMode.ForRead` - When only reading data
- `OpenMode.ForWrite` - When modifying objects

```csharp
// Reading
Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;

// Modifying
Entity ent = tr.GetObject(objId, OpenMode.ForWrite) as Entity;
ent.ColorIndex = 1; // Modify property
```

### 5. Upgrade Open Mode When Needed

```csharp
Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
// Later need to modify
ent.UpgradeOpen();
ent.ColorIndex = 1;
```

### 6. Handle ObjectId Collections Efficiently

```csharp
// Good - iterate directly
foreach (ObjectId objId in collection)
{
    Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
}

// Also good - convert to array if needed multiple times
ObjectId[] objIds = collection.Cast<ObjectId>().ToArray();
```

### 7. Use Type Checking with Pattern Matching (C# 7.0+)

```csharp
if (ent is Line line)
{
    // Use 'line' variable directly
    double length = line.Length;
}
```

### 8. Error Handling

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    try
    {
        // Your code here
        tr.Commit();
    }
    catch (Autodesk.AutoCAD.Runtime.Exception ex)
    {
        ed.WriteMessage($"\nAutoCAD Error: {ex.Message}");
        tr.Abort();
    }
    catch (System.Exception ex)
    {
        ed.WriteMessage($"\nGeneral Error: {ex.Message}");
        tr.Abort();
    }
}
```

### 9. Working with Both AutoCAD and Civil3D

```csharp
// Check if Civil3D is available
bool isCivil3D = CivilApplication.ActiveDocument != null;

if (isCivil3D)
{
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    // Work with Civil3D objects
}
else
{
    // Work with AutoCAD objects only
}
```

---

## Complete Example: Scanning All Objects

```csharp
[CommandMethod("SCANALL")]
public void ScanAllObjects()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Database db = acDoc.Database;
    Editor ed = acDoc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        try
        {
            // Scan AutoCAD entities
            BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
            BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
                OpenMode.ForRead) as BlockTableRecord;
            
            ed.WriteMessage("\n=== AutoCAD Entities ===");
            foreach (ObjectId objId in modelSpace)
            {
                Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
                ed.WriteMessage($"\n{ent.GetType().Name}");
            }
            
            // Scan Civil3D objects (if available)
            CivilDocument civilDoc = CivilDocument.GetCivilDocument(db);
            if (civilDoc != null)
            {
                ed.WriteMessage("\n\n=== Civil3D Objects ===");
                
                // Alignments
                foreach (ObjectId alignId in civilDoc.GetAlignmentIds())
                {
                    Alignment align = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
                    ed.WriteMessage($"\nAlignment: {align.Name}");
                }
                
                // Surfaces
                foreach (ObjectId surfId in civilDoc.GetSurfaceIds())
                {
                    Surface surf = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
                    ed.WriteMessage($"\nSurface: {surf.Name} ({surf.GetType().Name})");
                }
                
                // Add more Civil3D object types as needed...
            }
            
            tr.Commit();
        }
        catch (System.Exception ex)
        {
            ed.WriteMessage($"\nError: {ex.Message}");
            tr.Abort();
        }
    }
}
```

---

## References

- [AutoCAD .NET Developer's Guide](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Civil3D .NET API Reference](https://help.autodesk.com/view/CIV3D/2024/ENU/)
- [ObjectARX SDK Documentation](https://www.autodesk.com/developer-network/platform-technologies/autocad)
