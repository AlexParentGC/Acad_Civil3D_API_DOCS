# Classe Catchment

## Vue d'Ensemble
La classe `Catchment` représente un bassin versant dans Civil 3D, utilisé pour l'hydrologie et l'analyse de drainage.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Catchment
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom du bassin versant |
| `Area` | `double` | Obtient l'aire du bassin versant |
| `DischargePoint` | `Point3d` | Obtient/définit l'emplacement du point de rejet |

## Exemples de Code

### Exemple 1: Lister les Bassins Versants
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection catchmentIds = civilDoc.GetCatchmentIds();
    
    ed.WriteMessage($"\nTrouvé {catchmentIds.Count} bassins versants :");
    
    foreach (ObjectId catchmentId in catchmentIds)
    {
        Catchment catchment = tr.GetObject(catchmentId, OpenMode.ForRead) as Catchment;
        
        ed.WriteMessage($"\n  {catchment.Name}");
        ed.WriteMessage($"\n    Aire : {catchment.Area:F2} unités carrées");
        ed.WriteMessage($"\n    Point de Rejet : ({catchment.DischargePoint.X:F2}, {catchment.DischargePoint.Y:F2})");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Surface](../Surface/Surface.md) - Utilisé pour définir les limites du bassin versant
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour bassins versants

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Catchment)
