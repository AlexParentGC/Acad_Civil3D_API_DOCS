# Classe TinSurface

## Vue d'Ensemble
La classe `TinSurface` représente une surface TIN (Triangulated Irregular Network) dans Civil 3D, le type de surface le plus courant pour la modélisation de terrain.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ TinSurface
```

## Propriétés Clés

Hérite de toutes les propriétés de [Surface](Surface.md)

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetTriangles()` | `Triangle[]` | Obtient tous les triangles dans la surface |
| `GetPoints()` | `SurfacePoint[]` | Obtient tous les points de surface |

## Exemples de Code

### Exemple 1: Obtenir les Statistiques de Surface TIN
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    TinSurface tinSurf = tr.GetObject(surfaceId, OpenMode.ForRead) as TinSurface;
    
    GeneralSurfaceProperties props = tinSurf.GetGeneralProperties();
    
    ed.WriteMessage($"\nSurface TIN : {tinSurf.Name}");
    ed.WriteMessage($"\nNombre de Points : {props.NumberOfPoints}");
    ed.WriteMessage($"\nNombre de Triangles : {props.NumberOfTriangles}");
    ed.WriteMessage($"\nÉlévation Min : {props.MinimumElevation:F2}");
    ed.WriteMessage($"\nÉlévation Max : {props.MaximumElevation:F2}");
    
    tr.Commit();
}
```

## Objets Associés
- [Surface](Surface.md) - Classe de base
- [GridSurface](GridSurface.md) - Type de surface alternatif
- [CivilDocument](../Core/CivilDocument.md) - Conteneur

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-TinSurface)
