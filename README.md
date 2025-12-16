(Version francaise dans /fr)
# AutoCAD and Civil3D API Object Reference

## Quick Navigation

This directory contains comprehensive reference documentation for all AutoCAD and Civil3D .NET API object types, organized by category for easy searching.

---

## AutoCAD API

### Core Objects
Essential objects for working with AutoCAD documents and databases.

- [Database](AutoCAD/Core/Database.md) - Drawing database containing all objects
- [DBDictionary](AutoCAD/Core/DBDictionary.md) - Named object dictionary container
- [XRecord](AutoCAD/Core/XRecord.md) - Custom data storage in dictionaries
- [Document](AutoCAD/Core/Document.md) - Represents an open drawing document
- [Application](AutoCAD/Core/Application.md) - AutoCAD application object
- [Editor](AutoCAD/Core/Editor.md) - User interaction and command line

### User Interface
Customizing the AutoCAD UI.
- [PaletteSet](AutoCAD/UI/PaletteSet.md) - Dockable/Floating tool windows
- [Ribbon](AutoCAD/UI/Ribbon.md) - Ribbon tabs and panels (AdWindows)
- [ContextMenu](AutoCAD/UI/ContextMenu.md) - Custom right-click menus
- [SystemDialogs](AutoCAD/UI/SystemDialogs.md) - Open, Save, Color dialogs
- [StatusBar](AutoCAD/UI/StatusBar.md) - Progress meter and panes

### Base Classes
Fundamental base classes that other objects inherit from.

- [DBObject](AutoCAD/BaseClasses/DBObject.md) - Base class for all database objects
- [Entity](AutoCAD/BaseClasses/Entity.md) - Base class for all graphical entities
- [Curve](AutoCAD/BaseClasses/Curve.md) - Base class for all curve-based entities
- [ObjectId](AutoCAD/BaseClasses/ObjectId.md) - Persistent object identifier (Handle)
- [SymbolTable](AutoCAD/BaseClasses/SymbolTable.md) - Base container for named records
- [SymbolTableRecord](AutoCAD/BaseClasses/SymbolTableRecord.md) - Base item for symbol tables

### Collection Classes
Fundamental container classes used throughout the API.

- [ObjectIdCollection](AutoCAD/Collections/ObjectIdCollection.md) - List of ObjectIds
- [DBObjectCollection](AutoCAD/Collections/DBObjectCollection.md) - List of DBObjects
- [Point3dCollection](AutoCAD/Collections/Point3dCollection.md) - List of 3D points

### Geometry Classes
Fundamental geometry classes for 3D coordinates and calculations.

- [Point3d](AutoCAD/Geometry/Point3d.md) - 3D point coordinates (X, Y, Z)

### Symbol Tables
Collections of named objects like layers, linetypes, and text styles.

- [BlockTable](AutoCAD/SymbolTables/BlockTable.md) - Collection of block definitions
- [LayerTable](AutoCAD/SymbolTables/LayerTable.md) - Collection of layers
- [LinetypeTable](AutoCAD/SymbolTables/LinetypeTable.md) - Collection of linetypes
- [TextStyleTable](AutoCAD/SymbolTables/TextStyleTable.md) - Collection of text styles
- [DimStyleTable](AutoCAD/SymbolTables/DimStyleTable.md) - Collection of dimension styles
- [UcsTable](AutoCAD/SymbolTables/UcsTable.md) - Collection of user coordinate systems
- [ViewTable](AutoCAD/SymbolTables/ViewTable.md) - Collection of named views
- [ViewportTable](AutoCAD/SymbolTables/ViewportTable.md) - Collection of viewports
- [RegAppTable](AutoCAD/SymbolTables/RegAppTable.md) - Collection of registered applications

### Entities

#### Geometric Primitives
Basic geometric shapes.

- [Line](AutoCAD/Entities/Geometric/Line.md) - Simple line segment
- [Circle](AutoCAD/Entities/Geometric/Circle.md) - Circle object
- [Arc](AutoCAD/Entities/Geometric/Arc.md) - Arc segment
- [Ellipse](AutoCAD/Entities/Geometric/Ellipse.md) - Elliptical arc or full ellipse
- [Spline](AutoCAD/Entities/Geometric/Spline.md) - NURBS curve

### 3D Entities
3D modeling and surface objects.

- [Solid3d](AutoCAD/Entities/3D/Solid3d.md) - 3D solid (Box, Sphere, Boolean operations)
- [Region](AutoCAD/Entities/3D/Region.md) - 2D area with physical properties
- [Body](AutoCAD/Entities/3D/Body.md) - Generic ACIS body wrapper
- [SubDMesh](AutoCAD/Entities/3D/SubDMesh.md) - Subdivision mesh surface through points

#### Polylines
Multi-segment line entities.

- [Polyline](AutoCAD/Entities/Polylines/Polyline.md) - Lightweight 2D polyline
- [Polyline2d](AutoCAD/Entities/Polylines/Polyline2d.md) - Legacy 2D polyline
- [Polyline3d](AutoCAD/Entities/Polylines/Polyline3d.md) - 3D polyline

#### Text Objects
Text and annotation entities.

- [DBText](AutoCAD/Entities/Text/DBText.md) - Single-line text
- [MText](AutoCAD/Entities/Text/MText.md) - Multi-line text with formatting

#### Annotations
Dimensioning and leader objects.

- [Dimension](AutoCAD/Entities/Annotations/Dimension.md) - Base dimension class
- [Leader](AutoCAD/Entities/Annotations/Leader.md) - Leader line with annotation
- [MLeader](AutoCAD/Entities/Annotations/MLeader.md) - Multi-leader object

#### Complex Entities
Advanced entity types.

- [BlockReference](AutoCAD/Entities/Complex/BlockReference.md) - Block insertion
- [Hatch](AutoCAD/Entities/Complex/Hatch.md) - Filled area with pattern
- [Viewport](AutoCAD/Entities/Complex/Viewport.md) - Layout viewport

---

## Civil3D API

### Core
- [CivilApplication](Civil3D/Core/CivilApplication.md) - Civil3D application object
- [CivilDocument](Civil3D/Core/CivilDocument.md) - Civil3D document containing all Civil objects
- [Site](Civil3D/Core/Site.md) - Container for topology (Parcels, Alignments)

### Parcels
- [Parcel](Civil3D/Parcels/Parcel.md) - Land development parcels

### Alignment Objects
Horizontal and vertical alignment objects.

- [Alignment](Civil3D/Alignment/Alignment.md) - Horizontal alignment
- [Profile](Civil3D/Alignment/Profile.md) - Vertical profile

### Surface Objects
Terrain and volume surface objects.

- [Surface](Civil3D/Surface/Surface.md) - Base surface class
- [TinSurface](Civil3D/Surface/TinSurface.md) - Triangulated Irregular Network surface
- [GridSurface](Civil3D/Surface/GridSurface.md) - Grid-based surface
- [TinVolumeSurface](Civil3D/Surface/TinVolumeSurface.md) - TIN volume surface
- [GridVolumeSurface](Civil3D/Surface/GridVolumeSurface.md) - Grid volume surface

### Point Objects
Survey and COGO points.

- [CogoPoint](Civil3D/Points/CogoPoint.md) - Coordinate geometry point

### Pipe Network Objects
Storm and sanitary network objects.

- [Network](Civil3D/PipeNetworks/Network.md) - Pipe network container
- [Pipe](Civil3D/PipeNetworks/Pipe.md) - Pipe segment
- [Structure](Civil3D/PipeNetworks/Structure.md) - Manhole or inlet structure
- [PartsList](Civil3D/PipeNetworks/PartsList.md) - Catalog of pipe/structure parts

### Corridor Objects
Road corridor design objects.

- [Corridor](Civil3D/Corridor/Corridor.md) - Road corridor
- [Assembly](Civil3D/Corridor/Assembly.md) - Corridor assembly

### Grading Objects
Site grading and earthwork objects.

- [Grading](Civil3D/Grading/Grading.md) - Grading object
- [FeatureLine](Civil3D/Grading/FeatureLine.md) - 3D polyline for grading
- [Catchment](Civil3D/Grading/Catchment.md) - Drainage catchment area

### Sample Line Objects
Cross-section sampling objects.

- [SampleLine](Civil3D/SampleLines/SampleLine.md) - Cross-section sample line

---

## How to Use This Reference

1. **Find by Category**: Navigate through the folder structure to find objects by type
2. **Search by Name**: Use your IDE's file search to find specific object types
3. **Follow Links**: Each object page links to related objects
4. **Code Examples**: Every object includes practical code examples

## Additional Resources

- [AutoCAD and Civil3D API Workflow](../AutoCAD_Civil3D_API_Workflow.md) - Complete workflow guide
- [API Object Model Diagram](../AutoCAD_Civil3D_API_Object_Model.drawio) - Visual hierarchy
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)

---

## Legend

- 📁 **Core** - Essential application and document objects
- 📁 **BaseClasses** - Fundamental classes other objects inherit from
- 📁 **SymbolTables** - Named collections (layers, linetypes, etc.)
- 📁 **Entities** - Graphical objects in the drawing
- 📁 **Geometric** - Basic shapes (lines, circles, arcs)
- 📁 **Polylines** - Multi-segment line objects
- 📁 **Text** - Text and annotation
- 📁 **Annotations** - Dimensions and leaders
- 📁 **Complex** - Advanced entity types
