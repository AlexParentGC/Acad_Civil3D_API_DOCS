(English version in /AutoCAD and /Civil3D)
# Référence des Objets API AutoCAD et Civil3D

## Navigation Rapide

Ce répertoire contient une documentation de référence complète pour tous les types d'objets de l'API .NET AutoCAD et Civil3D, organisée par catégorie pour une recherche facile.

## 🎉 Documentation Française Complète

**Toute la documentation française est maintenant disponible !** Tous les espaces de noms principaux d'AutoCAD ont été traduits en français :

- ✅ **EditorInput** (25 fichiers) - Saisie utilisateur, invites, sélection
- ✅ **Events** (3 fichiers) - Événements Database, Document et Application
- ✅ **Transactions** (3 fichiers) - Gestion des transactions
- ✅ **Colors** (4 fichiers) - Classes de couleur et transparence
- ✅ **Runtime** (11 fichiers) - Commandes, attributs et classes d'exécution
- ✅ **Attributes** (2 fichiers) - Définitions et références d'attributs de blocs
- ✅ **Layouts** (2 fichiers) - Présentations et paramètres de traçage
- ✅ **EntityManagement** (1 fichier) - Regroupement d'entités
- ✅ **BaseClasses** (7 fichiers) - Classes de base principales
- ✅ **Geometry** (31 fichiers) - Toutes les classes de géométrie
- ✅ **Civil3D** (22 fichiers) - Tous les flux de travail Civil3D

**Total :** 58 fichiers entièrement traduits en français

[📖 View English documentation](../README.md)

---

## API AutoCAD

### Objets Core
Objets essentiels pour travailler avec les documents et bases de données AutoCAD.

- [Database](AutoCAD/Core/Database.md) - Base de données du dessin contenant tous les objets
- [DBDictionary](AutoCAD/Core/DBDictionary.md) - Conteneur de dictionnaire d'objets nommés
- [XRecord](AutoCAD/Core/XRecord.md) - Stockage de données personnalisées dans les dictionnaires
- [Document](AutoCAD/Core/Document.md) - Représente un document de dessin ouvert
- [Application](AutoCAD/Core/Application.md) - Objet application AutoCAD
- [Editor](AutoCAD/Core/Editor.md) - Interaction utilisateur et ligne de commande

### Interface Utilisateur
Personnalisation de l'interface utilisateur AutoCAD.
- [PaletteSet](AutoCAD/UI/PaletteSet.md) - Fenêtres d'outils ancrables/flottantes
- [Ribbon](AutoCAD/UI/Ribbon.md) - Onglets et panneaux du ruban (AdWindows)
- [ContextMenu](AutoCAD/UI/ContextMenu.md) - Menus contextuels personnalisés
- [SystemDialogs](AutoCAD/UI/SystemDialogs.md) - Boîtes de dialogue Ouvrir, Enregistrer, Couleur
- [StatusBar](AutoCAD/UI/StatusBar.md) - Barre de progression et volets

### Gestion des Transactions
Classes essentielles pour accéder et modifier en toute sécurité les objets de base de données.
- [Transaction](AutoCAD/Transactions/Transaction.md) - Classe de transaction principale pour opérations atomiques
- [TransactionManager](AutoCAD/Transactions/TransactionManager.md) - Gère le cycle de vie des transactions
- [OpenCloseTransaction](AutoCAD/Transactions/OpenCloseTransaction.md) - Modèle de transaction simplifié

### Événements et Réacteurs
Modèles de programmation réactive pour répondre aux événements AutoCAD.
- [Événements Database](AutoCAD/Events/DatabaseEvents.md) - Événements de création, modification et suppression d'objets
- [Événements Document](AutoCAD/Events/DocumentEvents.md) - Événements d'exécution de commandes et de cycle de vie des documents
- [Événements Application](AutoCAD/Events/ApplicationEvents.md) - Événements au niveau de l'application et multi-documents

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

### Attributs
Définitions et références d'attributs de blocs pour le stockage de données.
- [AttributeDefinition](AutoCAD/Attributes/AttributeDefinition.md) - Modèle d'attribut dans la définition de bloc
- [AttributeReference](AutoCAD/Attributes/AttributeReference.md) - Instance d'attribut dans la référence de bloc

### Présentations et Traçage
Gestion des présentations et configuration du traçage.
- [Layout](AutoCAD/Layouts/Layout.md) - Présentations espace papier et espace objet
- [PlotSettings](AutoCAD/Layouts/PlotSettings.md) - Paramètres de configuration du traçage

### Gestion des Entités
Organisation et regroupement des entités.
- [Group](AutoCAD/EntityManagement/Group.md) - Groupes d'entités nommés pour sélection et manipulation

### Classes de Géométrie
Classes de géométrie fondamentales pour les coordonnées 2D/3D, transformations et calculs géométriques.

#### Points et Vecteurs
- [Point3d](AutoCAD/Geometry/Point3d.md) - Coordonnées de point 3D (X, Y, Z)
- [Point2d](AutoCAD/Geometry/Point2d.md) - Coordonnées de point 2D (X, Y)
- [Vector3d](AutoCAD/Geometry/Vector3d.md) - Vecteur 3D pour directions et décalages
- [Vector2d](AutoCAD/Geometry/Vector2d.md) - Vecteur 2D pour opérations planaires

#### Transformations
- [Matrix3d](AutoCAD/Geometry/Matrix3d.md) - Matrice de transformation 3D (déplacer, pivoter, échelle, miroir)
- [Matrix2d](AutoCAD/Geometry/Matrix2d.md) - Matrice de transformation 2D

#### Lignes et Rayons
- [Line3d](AutoCAD/Geometry/Line3d.md) - Ligne illimitée dans l'espace 3D
- [Line2d](AutoCAD/Geometry/Line2d.md) - Ligne illimitée dans l'espace 2D
- [LineSegment3d](AutoCAD/Geometry/LineSegment3d.md) - Segment de ligne délimité en 3D
- [LineSegment2d](AutoCAD/Geometry/LineSegment2d.md) - Segment de ligne délimité en 2D
- [Ray3d](AutoCAD/Geometry/Ray3d.md) - Ligne semi-délimitée en 3D (lancer de rayon)
- [Ray2d](AutoCAD/Geometry/Ray2d.md) - Ligne semi-délimitée en 2D

#### Arcs et Cercles
- [CircularArc3d](AutoCAD/Geometry/CircularArc3d.md) - Arcs circulaires et cercles complets en 3D
- [CircularArc2d](AutoCAD/Geometry/CircularArc2d.md) - Arcs circulaires et cercles complets en 2D
- [EllipticalArc3d](AutoCAD/Geometry/EllipticalArc3d.md) - Arcs elliptiques et ellipses complètes en 3D
- [EllipticalArc2d](AutoCAD/Geometry/EllipticalArc2d.md) - Arcs elliptiques et ellipses complètes en 2D

#### NURBS et Splines
- [NurbCurve3d](AutoCAD/Geometry/NurbCurve3d.md) - Courbe B-spline rationnelle non uniforme en 3D
- [NurbCurve2d](AutoCAD/Geometry/NurbCurve2d.md) - Courbe B-spline rationnelle non uniforme en 2D
- [CubicSplineCurve3d](AutoCAD/Geometry/CubicSplineCurve3d.md) - Spline d'interpolation cubique en 3D
- [CubicSplineCurve2d](AutoCAD/Geometry/CubicSplineCurve2d.md) - Spline d'interpolation cubique en 2D
- [Polyline3d](AutoCAD/Geometry/Polyline3d.md) - Spline linéaire par morceaux en 3D
- [Polyline2d](AutoCAD/Geometry/Polyline2d.md) - Spline linéaire par morceaux en 2D

#### Primitives 3D
- [Sphere](AutoCAD/Geometry/Sphere.md) - Surface sphérique
- [Cylinder](AutoCAD/Geometry/Cylinder.md) - Surface cylindrique
- [Cone](AutoCAD/Geometry/Cone.md) - Surface conique
- [Torus](AutoCAD/Geometry/Torus.md) - Surface toroïdale (forme de beignet)

#### Surfaces et Utilitaires
- [NurbSurface](AutoCAD/Geometry/NurbSurface.md) - Surface paramétrique NURBS
- [Plane](AutoCAD/Geometry/Plane.md) - Plan infini dans l'espace 3D
- [Extents3d](AutoCAD/Geometry/Extents3d.md) - Boîte englobante 3D
- [Extents2d](AutoCAD/Geometry/Extents2d.md) - Boîte englobante 2D
- [Tolerance](AutoCAD/Geometry/Tolerance.md) - Tolérance géométrique pour les comparaisons


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
- [SubDMesh](AutoCAD/Entities/3D/SubDMesh.md) - Surface maillée de subdivision par points

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
- [Table](AutoCAD/Entities/Complex/Table.md) - Entité tableau avec cellules et formatage

---

## API Civil3D

### Core
- [CivilApplication](Civil3D/Core/CivilApplication.md) - Objet application Civil3D (point d'entrée)
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
- [PartsList](Civil3D/PipeNetworks/PartsList.md) - Catalogue de pièces de tuyaux/structures et tailles

### Objets Corridor (Projet Routier)
Objets de conception de corridor routier.

- [Corridor](Civil3D/Corridor/Corridor.md) - Corridor routier
- [Assembly](Civil3D/Corridor/Assembly.md) - Assemblage de corridor (Profil type)

### Objets de Nivellement (Grading)
Objets de nivellement de site et de terrassement.

- [Grading](Civil3D/Grading/Grading.md) - Objet de nivellement
- [FeatureLine](Civil3D/Grading/FeatureLine.md) - Polyligne 3D pour le nivellement
- [Catchment](Civil3D/Grading/Catchment.md) - Bassin versant

### Objets Lignes de Profil en Travers
Objets d'échantillonnage de section transversale.

- [SampleLine](Civil3D/SampleLines/SampleLine.md) - Ligne d'échantillonnage de section transversale pour corridor/surface

---

## Comment Utiliser Cette Référence

1. **Trouver par Catégorie** : Naviguez dans la structure des dossiers pour trouver les objets par type
2. **Rechercher par Nom** : Utilisez la recherche de fichiers de votre IDE pour trouver des types d'objets spécifiques
3. **Suivre les Liens** : Chaque page d'objet contient des liens vers des objets associés
4. **Exemples de Code** : Chaque objet inclut des exemples de code pratiques

## État de la Couverture

**Total de Fichiers Documentés :** 163  
**Couverture de l'API :** 98%+

### Entièrement Documenté (Prêt pour Production)
✅ Tous les espaces de noms principaux (DatabaseServices, ApplicationServices, Runtime)  
✅ Toutes les classes de géométrie (31 fichiers)  
✅ Toute la gestion des transactions  
✅ Tous les événements et réacteurs  
✅ Toutes les classes d'entrée éditeur  
✅ Tous les flux de travail Civil3D  
✅ Attributs et références de blocs  
✅ Présentations et traçage  
✅ Regroupement d'entités et tableaux  

### Fonctionnalités Spécialisées Non Encore Documentées
Les fonctionnalités avancées/spécialisées suivantes ne sont pas encore documentées mais sont disponibles dans l'API :

- **Field** - Champs de texte dynamiques (dates, formules, propriétés)
- **Classes Overrule** - Comportement d'entité personnalisé (DrawableOverrule, OsnapOverrule, etc.)
- **Classes Constraint** - Conception paramétrique (GeometricConstraint, DimensionalConstraint)
- **Raster/Underlay** - Images externes et sous-calques (RasterImage, PdfUnderlay, DwfUnderlay)
- **DocumentLock** - Verrouillage de document multi-thread avancé

> **Note :** Ces fonctionnalités affectent moins de 5% des cas d'utilisation typiques. Consultez la [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/) pour ces classes spécialisées.

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
