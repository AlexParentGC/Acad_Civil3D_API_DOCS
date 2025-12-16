# Référence des Objets API AutoCAD et Civil3D

## Navigation Rapide

Ce répertoire contient une documentation de référence complète pour tous les types d'objets de l'API .NET AutoCAD et Civil3D, organisée par catégorie pour une recherche facile.

---

## API AutoCAD

### Objets Core
Objets essentiels pour travailler avec les documents et bases de données AutoCAD.

- [Database](AutoCAD/Core/Database.md) - Base de données du dessin contenant tous les objets
- [DBDictionary](AutoCAD/Core/DBDictionary.md) - Dictionnaire d'objets nommés
- [XRecord](AutoCAD/Core/XRecord.md) - Stockage de données personnalisées dans les dictionnaires
- [Document](AutoCAD/Core/Document.md) - Représente un document de dessin ouvert
- [Application](AutoCAD/Core/Application.md) - Objet application AutoCAD
- [Editor](AutoCAD/Core/Editor.md) - Interaction utilisateur et ligne de commande

### Interface Utilisateur (UI)
Personnalisation de l'interface utilisateur AutoCAD.
- [PaletteSet](AutoCAD/UI/PaletteSet.md) - Fenêtres d'outils ancrables/flottantes
- [Ribbon](AutoCAD/UI/Ribbon.md) - Onglets et panneaux du ruban (AdWindows)
- [ContextMenu](AutoCAD/UI/ContextMenu.md) - Menus contextuels (clic droit) personnalisés
- [SystemDialogs](AutoCAD/UI/SystemDialogs.md) - Boîtes de dialogue Ouvrir, Enregistrer, Couleur
- [StatusBar](AutoCAD/UI/StatusBar.md) - Barre de progression et volets

### Classes de Base
Classes de base fondamentales dont héritent les autres objets.

- [DBObject](AutoCAD/BaseClasses/DBObject.md) - Classe de base pour tous les objets de base de données
- [Entity](AutoCAD/BaseClasses/Entity.md) - Classe de base pour toutes les entités graphiques
- [Curve](AutoCAD/BaseClasses/Curve.md) - Classe de base pour toutes les entités basées sur des courbes
- [ObjectId](AutoCAD/BaseClasses/ObjectId.md) - Identifiant d'objet persistant (Handle)
- [SymbolTable](AutoCAD/BaseClasses/SymbolTable.md) - Conteneur de base pour les enregistrements nommés
- [SymbolTableRecord](AutoCAD/BaseClasses/SymbolTableRecord.md) - Élément de base pour les tables de symboles

### Classes de Collection
Classes de conteneurs fondamentales utilisées dans toute l'API.

- [ObjectIdCollection](AutoCAD/Collections/ObjectIdCollection.md) - Liste d'ObjectIds
- [DBObjectCollection](AutoCAD/Collections/DBObjectCollection.md) - Liste de DBObjects
- [Point3dCollection](AutoCAD/Collections/Point3dCollection.md) - Liste de points 3D

### Classes de Géométrie
Classes de géométrie fondamentales pour les coordonnées et calculs 3D.

- [Point3d](AutoCAD/Geometry/Point3d.md) - Coordonnées de point 3D (X, Y, Z)

### Tables de Symboles
Collections d'objets nommés comme les calques, types de ligne et styles de texte.

- [BlockTable](AutoCAD/SymbolTables/BlockTable.md) - Collection de définitions de blocs
- [LayerTable](AutoCAD/SymbolTables/LayerTable.md) - Collection de calques
- [LinetypeTable](AutoCAD/SymbolTables/LinetypeTable.md) - Collection de types de ligne
- [TextStyleTable](AutoCAD/SymbolTables/TextStyleTable.md) - Collection de styles de texte
- [DimStyleTable](AutoCAD/SymbolTables/DimStyleTable.md) - Collection de styles de cotation
- [UcsTable](AutoCAD/SymbolTables/UcsTable.md) - Collection de systèmes de coordonnées utilisateur
- [ViewTable](AutoCAD/SymbolTables/ViewTable.md) - Collection de vues nommées
- [ViewportTable](AutoCAD/SymbolTables/ViewportTable.md) - Collection de fenêtres (viewports)
- [RegAppTable](AutoCAD/SymbolTables/RegAppTable.md) - Collection d'applications enregistrées

### Entités

#### Primitives Géométriques
Formes géométriques de base.

- [Line](AutoCAD/Entities/Geometric/Line.md) - Segment de ligne simple
- [Circle](AutoCAD/Entities/Geometric/Circle.md) - Objet cercle
- [Arc](AutoCAD/Entities/Geometric/Arc.md) - Segment d'arc
- [Ellipse](AutoCAD/Entities/Geometric/Ellipse.md) - Arc elliptique ou ellipse complète
- [Spline](AutoCAD/Entities/Geometric/Spline.md) - Courbe NURBS

### Entités 3D
Objets de modélisation 3D et de surface.

- [Solid3d](AutoCAD/Entities/3D/Solid3d.md) - Solide 3D (Boîte, Sphère, Opérations booléennes)
- [Region](AutoCAD/Entities/3D/Region.md) - Zone 2D avec propriétés physiques
- [Body](AutoCAD/Entities/3D/Body.md) - Enveloppe de corps ACIS générique
- [SubDMesh](AutoCAD/Entities/3D/SubDMesh.md) - Surface maillée de subdivision

#### Polylignes
Entités linéaires à segments multiples.

- [Polyline](AutoCAD/Entities/Polylines/Polyline.md) - Polyligne 2D légère
- [Polyline2d](AutoCAD/Entities/Polylines/Polyline2d.md) - Polyligne 2D héritée (Legacy)
- [Polyline3d](AutoCAD/Entities/Polylines/Polyline3d.md) - Polyligne 3D

#### Objets Texte
Entités de texte et d'annotation.

- [DBText](AutoCAD/Entities/Text/DBText.md) - Texte sur une seule ligne
- [MText](AutoCAD/Entities/Text/MText.md) - Texte multi-lignes avec formatage

#### Annotations
Objets de cotation et lignes de repère.

- [Dimension](AutoCAD/Entities/Annotations/Dimension.md) - Classe de base pour les dimensions
- [Leader](AutoCAD/Entities/Annotations/Leader.md) - Ligne de repère avec annotation
- [MLeader](AutoCAD/Entities/Annotations/MLeader.md) - Objet multi-repère

#### Entités Complexes
Types d'entités avancés.

- [BlockReference](AutoCAD/Entities/Complex/BlockReference.md) - Insertion de bloc
- [Hatch](AutoCAD/Entities/Complex/Hatch.md) - Zone remplie avec motif
- [Viewport](AutoCAD/Entities/Complex/Viewport.md) - Fenêtre de présentation

---

## API Civil3D

### Core
- [CivilApplication](Civil3D/Core/CivilApplication.md) - Objet application Civil3D
- [CivilDocument](Civil3D/Core/CivilDocument.md) - Document Civil3D contenant tous les objets Civil
- [Site](Civil3D/Core/Site.md) - Conteneur pour la topologie (Parcelles, Alignements)

### Parcelles
- [Parcel](Civil3D/Parcels/Parcel.md) - Parcelles d'aménagement du territoire

### Objets d'Alignement
Objets d'alignement horizontal et vertical.

- [Alignment](Civil3D/Alignment/Alignment.md) - Alignement horizontal
- [Profile](Civil3D/Alignment/Profile.md) - Profil vertical

### Objets de Surface
Objets de terrain et de surface volumique.

- [Surface](Civil3D/Surface/Surface.md) - Classe de base pour les surfaces
- [TinSurface](Civil3D/Surface/TinSurface.md) - Surface de réseau irrégulier triangulé (TIN)
- [GridSurface](Civil3D/Surface/GridSurface.md) - Surface basée sur une grille
- [TinVolumeSurface](Civil3D/Surface/TinVolumeSurface.md) - Surface volumique TIN
- [GridVolumeSurface](Civil3D/Surface/GridVolumeSurface.md) - Surface volumique de grille

### Objets Points
Points de topographie et COGO.

- [CogoPoint](Civil3D/Points/CogoPoint.md) - Point de géométrie de coordonnées

### Objets de Réseau de Canalisations
Objets de réseaux pluviaux et sanitaires.

- [Network](Civil3D/PipeNetworks/Network.md) - Conteneur de réseau de canalisations
- [Pipe](Civil3D/PipeNetworks/Pipe.md) - Segment de tuyau
- [Structure](Civil3D/PipeNetworks/Structure.md) - Structure de regard ou d'entrée
- [PartsList](Civil3D/PipeNetworks/PartsList.md) - Catalogue de pièces (tuyaux/structures)

### Objets Corridor (Projet Routier)
Objets de conception de corridor routier.

- [Corridor](Civil3D/Corridor/Corridor.md) - Corridor routier
- [Assembly](Civil3D/Corridor/Assembly.md) - Assemblage de corridor (Profil type)

### Objets de Nivellement (Grading)
Objets de nivellement de site et de terrassement.

- [Grading](Civil3D/Grading/Grading.md) - Objet de nivellement
- [FeatureLine](Civil3D/Grading/FeatureLine.md) - Polyligne 3D pour le nivellement
- [Catchment](Civil3D/Grading/Catchment.md) - Bassin versant

### Objets Lignes de Profil en Travers (Sample Line)
Objets d'échantillonnage de section transversale.

- [SampleLine](Civil3D/SampleLines/SampleLine.md) - Ligne d'échantillonnage de section transversale

---

## Comment Utiliser Cette Référence

1. **Trouver par Catégorie** : Naviguez dans la structure des dossiers pour trouver les objets par type
2. **Rechercher par Nom** : Utilisez la recherche de fichiers de votre IDE pour trouver des types d'objets spécifiques
3. **Suivre les Liens** : Chaque page d'objet contient des liens vers des objets associés
4. **Exemples de Code** : Chaque objet inclut des exemples de code pratiques

## Ressources Supplémentaires

- [Workflow API AutoCAD et Civil3D](../AutoCAD_Civil3D_API_Workflow.md) - Guide complet du workflow
- [Diagramme du Modèle Objet API](../AutoCAD_Civil3D_API_Object_Model.drawio) - Hiérarchie visuelle
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)

---

## Légende

- 📁 **Core** - Objets essentiels de l'application et du document
- 📁 **BaseClasses** - Classes fondamentales dont héritent les autres objets
- 📁 **SymbolTables** - Collections nommées (calques, types de ligne, etc.)
- 📁 **Entities** - Objets graphiques dans le dessin
- 📁 **Geometric** - Formes de base (lignes, cercles, arcs)
- 📁 **Polylines** - Objets linéaires à segments multiples
- 📁 **Text** - Texte et annotation
- 📁 **Annotations** - Cotes et lignes de repère
- 📁 **Complex** - Types d'entités avancés
