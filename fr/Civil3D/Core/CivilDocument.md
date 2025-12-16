# Classe CivilDocument

## Vue d'Ensemble
La classe `CivilDocument` est l'objet central pour accéder à tous les objets spécifiques à Civil 3D dans un dessin. Elle fournit des collections et des méthodes pour travailler avec les axes, surfaces, réseaux de canalisations et autres entités Civil 3D.

## Namespace
`Autodesk.Civil.ApplicationServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ CivilDocument
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Database` | `Database` | Obtient la base de données AutoCAD associée |
| `Name` | `string` | Obtient le nom du document |
| `Settings` | `SettingsCivil3D` | Obtient les paramètres Civil 3D |
| `Styles` | `StylesRoot` | Obtient la racine des styles Civil 3D |

## Méthodes Clés - Collections d'Objets

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetAlignmentIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds d'axes |
| `GetSurfaceIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de surfaces |
| `GetPipeNetworkIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de réseaux de canalisations |
| `GetCorridorIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de projets 3D |
| `GetAssemblyIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds d'assemblages |
| `GetCatchmentIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de bassins versants |
| `GetGradingIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de terrassements |
| `GetFeatureLineIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de lignes caractéristiques |
| `GetSampleLineGroupIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de groupes de lignes de profil en travers |

## Exemples de Code

### Exemple 1: Obtenir CivilDocument
```csharp
using Autodesk.Civil.ApplicationServices;
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
CivilDocument civilDoc = CivilDocument.GetCivilDocument(acDoc.Database);

// Ou utiliser le document actif directement
CivilDocument civilDoc2 = CivilApplication.ActiveDocument;
```

### Exemple 2: Lister Tous les Axes
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    
    ed.WriteMessage($"\nTrouvé {alignmentIds.Count} axes :");
    
    foreach (ObjectId alignId in alignmentIds)
    {
        Alignment alignment = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
        ed.WriteMessage($"\n  {alignment.Name} - Longueur : {alignment.Length:F2}");
    }
    
    tr.Commit();
}
```

### Exemple 3: Lister Toutes les Surfaces
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    
    ed.WriteMessage($"\nTrouvé {surfaceIds.Count} surfaces :");
    
    foreach (ObjectId surfId in surfaceIds)
    {
        Surface surface = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
        
        string surfaceType = surface.GetType().Name;
        ed.WriteMessage($"\n  {surface.Name} ({surfaceType})");
        
        if (surface is TinSurface tinSurf)
        {
            ed.WriteMessage($" - Points : {tinSurf.GetGeneralProperties().NumberOfPoints}");
        }
    }
    
    tr.Commit();
}
```

### Exemple 4: Travailler avec des Points COGO
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    CogoPointCollection cogoPoints = civilDoc.CogoPoints;
    
    // Obtenir tous les numéros de points
    ObjectIdCollection pointIds = cogoPoints.GetPointIds();
    
    ed.WriteMessage($"\nTrouvé {pointIds.Count} points COGO :");
    
    foreach (ObjectId pointId in pointIds)
    {
        CogoPoint point = tr.GetObject(pointId, OpenMode.ForRead) as CogoPoint;
        ed.WriteMessage($"\n  Point {point.PointNumber}: ({point.Easting:F2}, {point.Northing:F2}, {point.Elevation:F2})");
    }
    
    tr.Commit();
}
```

### Exemple 5: Accéder aux Réseaux de Canalisations
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection networkIds = civilDoc.GetPipeNetworkIds();
    
    foreach (ObjectId networkId in networkIds)
    {
        Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
        
        ed.WriteMessage($"\nRéseau : {network.Name}");
        
        // Obtenir tuyaux dans le réseau
        ObjectIdCollection pipeIds = network.GetPipeIds();
        ed.WriteMessage($"\n  Tuyaux : {pipeIds.Count}");
        
        // Obtenir structures dans le réseau
        ObjectIdCollection structureIds = network.GetStructureIds();
        ed.WriteMessage($"\n  Structures : {structureIds.Count}");
    }
    
    tr.Commit();
}
```

## Modèles Communs

### Vérifier si les Objets Civil 3D Existent
```csharp
Document acDoc = Application.DocumentManager.MdiActiveDocument;
CivilDocument civilDoc = null;

try
{
    civilDoc = CivilDocument.GetCivilDocument(acDoc.Database);
    
    if (civilDoc != null)
    {
        // Civil 3D est disponible
        ObjectIdCollection alignments = civilDoc.GetAlignmentIds();
        bool hasAlignments = alignments.Count > 0;
    }
}
catch (System.Exception)
{
    // Civil 3D non disponible ou pas d'objets Civil 3D
}
```

### Itérer à Travers Tous les Types d'Objets Civil 3D
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ed.WriteMessage("\n=== Résumé Objets Civil 3D ===");
    ed.WriteMessage($"\nAxes : {civilDoc.GetAlignmentIds().Count}");
    ed.WriteMessage($"\nSurfaces : {civilDoc.GetSurfaceIds().Count}");
    ed.WriteMessage($"\nRéseaux : {civilDoc.GetPipeNetworkIds().Count}");
    ed.WriteMessage($"\nProjets 3D : {civilDoc.GetCorridorIds().Count}");
    ed.WriteMessage($"\nAssemblages : {civilDoc.GetAssemblyIds().Count}");
    ed.WriteMessage($"\nBassins Versants : {civilDoc.GetCatchmentIds().Count}");
    ed.WriteMessage($"\nTerrassements : {civilDoc.GetGradingIds().Count}");
    ed.WriteMessage($"\nLignes Caractéristiques : {civilDoc.GetFeatureLineIds().Count}");
    ed.WriteMessage($"\nPoints COGO : {civilDoc.CogoPoints.Count}");
    
    tr.Commit();
}
```

## Objets Associés
- [CivilApplication](CivilApplication.md) - Objet application Civil 3D
- [Alignment](../Alignment/Alignment.md) - Axe horizontal
- [Surface](../Surface/Surface.md) - Surface topographique
- [Network](../PipeNetworks/Network.md) - Réseau de canalisations
- [CogoPoint](../Points/CogoPoint.md) - Point topographique

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-CivilDocument)
