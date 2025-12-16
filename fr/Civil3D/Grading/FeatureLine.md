# Classe FeatureLine

## Vue d'Ensemble
La classe `FeatureLine` représente une ligne caractéristique (polyligne 3D améliorée) utilisée pour le terrassement et la conception de projets 3D dans Civil 3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ FeatureLine
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom de la ligne caractéristique |
| `Length2D` | `double` | Obtient la longueur 2D (horizontale) |
| `Length3D` | `double` | Obtient la longueur 3D (pente) |
| `MaximumElevation` | `double` | Obtient l'élévation maximale |
| `MinimumElevation` | `double` | Obtient l'élévation minimale |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetPoints(FeatureLinePointType)` | `Point3dCollection` | Obtient les points le long de la ligne caractéristique |
| `ElevationAtPoint(Point3d)` | `double` | Obtient l'élévation à un point |

## Exemples de Code

### Exemple 1: Accéder aux Lignes Caractéristiques
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection featureLineIds = civilDoc.GetFeatureLineIds();
    
    foreach (ObjectId featureLineId in featureLineIds)
    {
        FeatureLine featureLine = tr.GetObject(featureLineId, OpenMode.ForRead) as FeatureLine;
        
        ed.WriteMessage($"\nLigne Caractéristique : {featureLine.Name}");
        ed.WriteMessage($"\n  Longueur 2D : {featureLine.Length2D:F2}");
        ed.WriteMessage($"\n  Longueur 3D : {featureLine.Length3D:F2}");
        ed.WriteMessage($"\n  Élévation Min : {featureLine.MinimumElevation:F2}");
        ed.WriteMessage($"\n  Élévation Max : {featureLine.MaximumElevation:F2}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Grading](Grading.md) - Utilise les lignes caractéristiques
- [Corridor](../Corridor/Corridor.md) - Peut extraire des lignes caractéristiques
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour lignes caractéristiques

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-FeatureLine)
