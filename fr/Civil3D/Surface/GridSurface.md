# Classe GridSurface

## Vue d'Ensemble
La classe `GridSurface` représente une surface basée sur une grille dans Civil 3D, une alternative aux surfaces TIN utilisant une grille régulière de points d'élévation.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Surface
                  └─ GridSurface
```

## Propriétés Clés

Hérite de toutes les propriétés de [Surface](Surface.md)

## Exemples de Code

### Exemple 1: Accéder à une Surface Grille
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    
    foreach (ObjectId surfId in surfaceIds)
    {
        Surface surface = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
        
        if (surface is GridSurface gridSurf)
        {
            GeneralSurfaceProperties props = gridSurf.GetGeneralProperties();
            
            ed.WriteMessage($"\nSurface Grille : {gridSurf.Name}");
            ed.WriteMessage($"\nÉlévation Min : {props.MinimumElevation:F2}");
            ed.WriteMessage($"\nÉlévation Max : {props.MaximumElevation:F2}");
            ed.WriteMessage($"\nAire 2D : {props.Area2d:F2}");
        }
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Surface](Surface.md) - Classe de base
- [TinSurface](TinSurface.md) - Type de surface alternatif
- [CivilDocument](../Core/CivilDocument.md) - Conteneur

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-GridSurface)
