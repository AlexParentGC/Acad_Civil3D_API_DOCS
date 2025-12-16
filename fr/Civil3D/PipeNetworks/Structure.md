# Classe Structure

## Vue d'Ensemble
La classe `Structure` représente une structure (regard, avaloir, exutoire, etc.) dans un réseau de canalisations Civil 3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Structure
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient le nom de la structure |
| `NetworkId` | `ObjectId` | Obtient l'ObjectId du réseau parent |
| `Location` | `Point3d` | Obtient l'emplacement 3D |
| `SumpElevation` | `double` | Obtient/définit l'élévation du radier |
| `RimElevation` | `double` | Obtient/définit l'élévation du tampon |
| `ConnectedPipesCount` | `int` | Obtient le nombre de tuyaux connectés |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetConnectedPipeIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds des tuyaux connectés |

## Exemples de Code

### Exemple 1: Lister les Propriétés de Structure
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
    ObjectIdCollection structureIds = network.GetStructureIds();
    
    foreach (ObjectId structId in structureIds)
    {
        Structure structure = tr.GetObject(structId, OpenMode.ForRead) as Structure;
        
        ed.WriteMessage($"\nStructure : {structure.Name}");
        ed.WriteMessage($"\n  Emplacement : ({structure.Location.X:F2}, {structure.Location.Y:F2})");
        ed.WriteMessage($"\n  Élévation Tampon : {structure.RimElevation:F2}");
        ed.WriteMessage($"\n  Élévation Radier : {structure.SumpElevation:F2}");
        ed.WriteMessage($"\n  Tuyaux Connectés : {structure.ConnectedPipesCount}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Network](Network.md) - Réseau de canalisations parent
- [Pipe](Pipe.md) - Tuyaux connectés
- [CivilDocument](../Core/CivilDocument.md) - Conteneur de document

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Structure)
