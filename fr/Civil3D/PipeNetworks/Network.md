# Classe Network

## Vue d'Ensemble
La classe `Network` représente un réseau de canalisations dans Civil 3D, contenant des tuyaux et des structures pour les systèmes pluviaux ou sanitaires.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Network
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom du réseau |
| `Description` | `string` | Obtient/définit la description |
| `ReferenceAlignmentId` | `ObjectId` | Obtient/définit l'axe de référence |
| `ReferenceSurfaceId` | `ObjectId` | Obtient/définit la surface de référence |
| `PartsListId` | `ObjectId` | Obtient/définit la liste de pièces |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetPipeIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de tuyaux dans le réseau |
| `GetStructureIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de structures dans le réseau |

## Exemples de Code

### Exemple 1: Accéder au Réseau de Canalisations
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection networkIds = civilDoc.GetPipeNetworkIds();
    
    foreach (ObjectId networkId in networkIds)
    {
        Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
        
        ed.WriteMessage($"\nRéseau : {network.Name}");
        
        ObjectIdCollection pipeIds = network.GetPipeIds();
        ObjectIdCollection structureIds = network.GetStructureIds();
        
        ed.WriteMessage($"\n  Tuyaux : {pipeIds.Count}");
        ed.WriteMessage($"\n  Structures : {structureIds.Count}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Pipe](Pipe.md) - Segments de tuyaux dans le réseau
- [Structure](Structure.md) - Regards et avaloirs dans le réseau
- PartsList - Catalogue de pièces de tuyaux/structures
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour réseaux

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Network)
