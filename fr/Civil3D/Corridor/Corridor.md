# Classe Corridor

## Vue d'Ensemble
La classe `Corridor` représente un projet 3D (corridor) dans Civil 3D, qui modélise une conception routière utilisant des axes, des profils et des assemblages.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Corridor
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom du projet 3D |
| `Description` | `string` | Obtient/définit la description |
| `AlignmentId` | `ObjectId` | Obtient/définit l'axe de base |
| `ProfileId` | `ObjectId` | Obtient/définit le profil de base |
| `StyleId` | `ObjectId` | Obtient/définit le style du projet 3D |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetBaselines()` | `BaselineCollection` | Obtient la collection des lignes de base |
| `Rebuild()` | `void` | Reconstruit le projet 3D |

## Exemples de Code

### Exemple 1: Accéder aux Propriétés du Projet 3D
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection corridorIds = civilDoc.GetCorridorIds();
    
    foreach (ObjectId corridorId in corridorIds)
    {
        Corridor corridor = tr.GetObject(corridorId, OpenMode.ForRead) as Corridor;
        
        ed.WriteMessage($"\nProjet 3D : {corridor.Name}");
        ed.WriteMessage($"\nDescription : {corridor.Description}");
        
        // Obtenir lignes de base
        BaselineCollection baselines = corridor.GetBaselines();
        ed.WriteMessage($"\nNombre de Lignes de Base : {baselines.Count}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Itérer à Travers les Lignes de Base
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Corridor corridor = tr.GetObject(corridorId, OpenMode.ForRead) as Corridor;
    
    BaselineCollection baselines = corridor.GetBaselines();
    
    foreach (Baseline baseline in baselines)
    {
        ed.WriteMessage($"\nLigne de Base : {baseline.Name}");
        ed.WriteMessage($"\n  Station Début : {baseline.StartStation:F2}");
        ed.WriteMessage($"\n  Station Fin : {baseline.EndStation:F2}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Assembly](Assembly.md) - Modèle de section transversale du projet 3D
- [Alignment](../Alignment/Alignment.md) - Axe de ligne de base
- [Profile](../Alignment/Profile.md) - Profil de ligne de base
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour projets 3D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Corridor)
