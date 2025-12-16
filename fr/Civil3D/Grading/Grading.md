# Classe Grading

## Vue d'Ensemble
La classe `Grading` représente un objet de terrassement dans Civil 3D, utilisé pour le nivellement de site et la conception de terrassement.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Grading
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient le nom du terrassement |
| `SurfaceId` | `ObjectId` | Obtient/définit la surface cible |
| `StyleId` | `ObjectId` | Obtient/définit le style de terrassement |

## Exemples de Code

### Exemple 1: Lister les Terrassements
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection gradingIds = civilDoc.GetGradingIds();
    
    ed.WriteMessage($"\nTrouvé {gradingIds.Count} objets de terrassement :");
    
    foreach (ObjectId gradingId in gradingIds)
    {
        Grading grading = tr.GetObject(gradingId, OpenMode.ForRead) as Grading;
        
        ed.WriteMessage($"\n  {grading.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Surface](../Surface/Surface.md) - Surface cible pour le terrassement
- [FeatureLine](FeatureLine.md) - Souvent utilisé avec le terrassement
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour terrassements

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Grading)
