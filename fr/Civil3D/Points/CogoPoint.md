# Classe CogoPoint

## Vue d'Ensemble
La classe `CogoPoint` représente un point de géométrie de coordonnées (COGO) dans Civil 3D, utilisé pour les points de levé et l'implantation de site.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ CogoPoint
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `PointNumber` | `uint` | Obtient/définit le numéro de point |
| `Easting` | `double` | Obtient/définit la coordonnée X (Abscisse) |
| `Northing` | `double` | Obtient/définit la coordonnée Y (Ordonnée) |
| `Elevation` | `double` | Obtient/définit la coordonnée Z (Élévation) |
| `RawDescription` | `string` | Obtient/définit la description brute |
| `FullDescription` | `string` | Obtient la description complète avec codes |
| `Location` | `Point3d` | Obtient/définit l'emplacement 3D |

## Exemples de Code

### Exemple 1: Accéder aux Points COGO
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    CogoPointCollection cogoPoints = civilDoc.CogoPoints;
    ObjectIdCollection pointIds = cogoPoints.GetPointIds();
    
    foreach (ObjectId pointId in pointIds)
    {
        CogoPoint point = tr.GetObject(pointId, OpenMode.ForRead) as CogoPoint;
        
        ed.WriteMessage($"\nPoint {point.PointNumber} :");
        ed.WriteMessage($"\n  E : {point.Easting:F3}");
        ed.WriteMessage($"\n  N : {point.Northing:F3}");
        ed.WriteMessage($"\n  Elev : {point.Elevation:F3}");
        ed.WriteMessage($"\n  Desc : {point.RawDescription}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Créer un Point COGO
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    CogoPointCollection cogoPoints = civilDoc.CogoPoints;
    
    // Ajouter un nouveau point
    ObjectId pointId = cogoPoints.Add(new Point3d(1000, 2000, 150), true);
    
    CogoPoint newPoint = tr.GetObject(pointId, OpenMode.ForWrite) as CogoPoint;
    newPoint.PointNumber = 100;
    newPoint.RawDescription = "Coin Propriété";
    
    tr.Commit();
}
```

## Objets Associés
- [CivilDocument](../Core/CivilDocument.md) - Contient CogoPointCollection
- [Surface](../Surface/Surface.md) - Peut être créé à partir de points COGO

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-CogoPoint)
