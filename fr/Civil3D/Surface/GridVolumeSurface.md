# Classe GridVolumeSurface

## Vue d'Ensemble
La classe `GridVolumeSurface` représente une surface différentielle (volume) basée sur une grille dans Civil 3D, utilisée pour calculer les volumes de déblai et remblai entre deux surfaces grille.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ GridVolumeSurface
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `BaseSurfaceId` | `ObjectId` | Obtient/définit la surface de base |
| `ComparisonSurfaceId` | `ObjectId` | Obtient/définit la surface de comparaison |

## Exemples de Code

### Exemple 1: Accéder à une Surface Volume Grille
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    GridVolumeSurface volSurf = tr.GetObject(volumeSurfaceId, OpenMode.ForRead) as GridVolumeSurface;
    
    ed.WriteMessage($"\nSurface Volume Grille : {volSurf.Name}");
    
    Surface baseSurf = tr.GetObject(volSurf.BaseSurfaceId, OpenMode.ForRead) as Surface;
    Surface compSurf = tr.GetObject(volSurf.ComparisonSurfaceId, OpenMode.ForRead) as Surface;
    
    ed.WriteMessage($"\nSurface Base : {baseSurf.Name}");
    ed.WriteMessage($"\nSurface Comparaison : {compSurf.Name}");
    
    tr.Commit();
}
```

## Objets Associés
- [Surface](Surface.md) - Classe de base
- [GridSurface](GridSurface.md) - Surfaces composantes
- [CivilDocument](../Core/CivilDocument.md) - Conteneur

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-GridVolumeSurface)
