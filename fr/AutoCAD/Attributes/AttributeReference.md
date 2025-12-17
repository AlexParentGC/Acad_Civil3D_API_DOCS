# AttributeReference

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `AttributeReference` représente une instance d'attribut dans une référence de bloc insérée. Elle contient la valeur réelle de l'attribut basée sur sa définition.

**Concept Clé:** Lorsqu'un bloc avec AttributeDefinitions est inséré, des AttributeReferences sont créées pour chaque définition, contenant les valeurs réelles des attributs.

## Hiérarchie de Classe

```
Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ DBText
                  └─ AttributeReference
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Tag` | `string` | Obtient/définit l'étiquette d'attribut |
| `TextString` | `string` | Obtient/définit la valeur de l'attribut |
| `Invisible` | `bool` | Obtient/définit si l'attribut est invisible |
| `IsMTextAttribute` | `bool` | Vérifie si l'attribut est multilignes |

## Exemples de Code

### Lecture des Valeurs d'Attributs

```csharp
using Autodesk.AutoCAD.DatabaseServices;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockReference blockRef = tr.GetObject(blockRefId, OpenMode.ForRead) as BlockReference;
    
    AttributeCollection attCol = blockRef.AttributeCollection;
    
    foreach (ObjectId attId in attCol)
    {
        AttributeReference attRef = tr.GetObject(attId, OpenMode.ForRead) as AttributeReference;
        
        ed.WriteMessage($"\nÉtiquette : {attRef.Tag}");
        ed.WriteMessage($"\nValeur : {attRef.TextString}");
    }
    
    tr.Commit();
}
```

### Modification des Valeurs d'Attributs

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockReference blockRef = tr.GetObject(blockRefId, OpenMode.ForRead) as BlockReference;
    
    AttributeCollection attCol = blockRef.AttributeCollection;
    
    foreach (ObjectId attId in attCol)
    {
        AttributeReference attRef = tr.GetObject(attId, OpenMode.ForWrite) as AttributeReference;
        
        if (attRef.Tag == "NUMERO_PIECE")
        {
            attRef.TextString = "NOUVELLE_VALEUR";
        }
    }
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Vérifier l'Étiquette**: Toujours vérifier l'étiquette avant de modifier
2. **Utiliser des Transactions**: Toujours utiliser des transactions
3. **Gérer les Attributs Invisibles**: Les attributs invisibles existent toujours
4. **Synchroniser avec la Définition**: Les modifications n'affectent pas la définition

## Classes Associées

- [AttributeDefinition](AttributeDefinition.md) - Modèle d'attribut
- [BlockReference](../Entities/Complex/BlockReference.md) - Référence de bloc
- [AttributeCollection](../Collections/AttributeCollection.md) - Collection d'attributs

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
