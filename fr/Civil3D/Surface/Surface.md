# Classe Surface

## Vue d'Ensemble
La classe `Surface` est la classe de base pour tous les types de surfaces Civil 3D, incluant les surfaces TIN, les surfaces grille, et les surfaces différentielles (volume).

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  ├─ TinSurface
                  ├─ GridSurface
                  ├─ TinVolumeSurface
                  └─ GridVolumeSurface
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom de la surface |
| `Description` | `string` | Obtient/définit la description |
| `StyleId` | `ObjectId` | Obtient/définit le style de surface |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `FindElevationAtXY(double, double)` | `double` | Obtient l'élévation aux coordonnées XY |
| `SampleElevations(Point2dCollection)` | `Point3dCollection` | Obtient les élévations à plusieurs points |
| `GetGeneralProperties()` | `GeneralSurfaceProperties` | Obtient les statistiques de surface |

## Exemples de Code

### Exemple 1: Obtenir l'Élévation de Surface
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    
    if (surfaceIds.Count > 0)
    {
        Surface surface = tr.GetObject(surfaceIds[0], OpenMode.ForRead) as Surface;
        
        double x = 1000.0;
        double y = 2000.0;
        
        try
        {
            double elevation = surface.FindElevationAtXY(x, y);
            ed.WriteMessage($"\nÉlévation à ({x}, {y}) : {elevation:F2}");
        }
        catch
        {
            ed.WriteMessage("\nLe point est hors des limites de la surface");
        }
    }
    
    tr.Commit();
}
```

### Exemple 2: Obtenir les Propriétés de Surface
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Surface surface = tr.GetObject(surfaceId, OpenMode.ForRead) as Surface;
    
    GeneralSurfaceProperties props = surface.GetGeneralProperties();
    
    ed.WriteMessage($"\nSurface : {surface.Name}");
    ed.WriteMessage($"\nÉlévation Min : {props.MinimumElevation:F2}");
    ed.WriteMessage($"\nÉlévation Max : {props.MaximumElevation:F2}");
    ed.WriteMessage($"\nAire 2D : {props.Area2d:F2}");
    ed.WriteMessage($"\nAire 3D : {props.Area3d:F2}");
    
    tr.Commit();
}
```

## Objets Associés
- [TinSurface](TinSurface.md) - Surface triangulée
- [GridSurface](GridSurface.md) - Surface grille
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour surfaces

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Surface)
