# Classe Pipe

## Vue d'Ensemble
La classe `Pipe` représente un segment de tuyau dans un réseau de canalisations Civil 3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Pipe
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient le nom du tuyau |
| `NetworkId` | `ObjectId` | Obtient l'ObjectId du réseau parent |
| `StartPoint` | `Point3d` | Obtient le point de départ (3D) |
| `EndPoint` | `Point3d` | Obtient le point de fin (3D) |
| `Length2D` | `double` | Obtient la longueur 2D (horizontale) |
| `Length3D` | `double` | Obtient la longueur 3D (pente) |
| `InnerDiameterOrWidth` | `double` | Obtient/définit le diamètre intérieur |
| `OuterDiameterOrWidth` | `double` | Obtient le diamètre extérieur |
| `Slope` | `double` | Obtient la pente (montée/course) |
| `StartOffset` | `double` | Obtient/définit le décalage du radier de départ |
| `EndOffset` | `double` | Obtient/définit le décalage du radier de fin |
| `FlowDirection` | `FlowDirectionType` | Obtient/définit la direction d'écoulement |

## Exemples de Code

### Exemple 1: Lister les Propriétés de Tuyau
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
    ObjectIdCollection pipeIds = network.GetPipeIds();
    
    foreach (ObjectId pipeId in pipeIds)
    {
        Pipe pipe = tr.GetObject(pipeId, OpenMode.ForRead) as Pipe;
        
        ed.WriteMessage($"\nTuyau : {pipe.Name}");
        ed.WriteMessage($"\n  Diamètre : {pipe.InnerDiameterOrWidth:F2}");
        ed.WriteMessage($"\n  Longueur 2D : {pipe.Length2D:F2}");
        ed.WriteMessage($"\n  Longueur 3D : {pipe.Length3D:F2}");
        ed.WriteMessage($"\n  Pente : {pipe.Slope * 100:F2}%");
        ed.WriteMessage($"\n  Direction Écoulement : {pipe.FlowDirection}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Network](Network.md) - Réseau de canalisations parent
- [Structure](Structure.md) - Structures connectées
- [CivilDocument](../Core/CivilDocument.md) - Conteneur de document

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Pipe)
