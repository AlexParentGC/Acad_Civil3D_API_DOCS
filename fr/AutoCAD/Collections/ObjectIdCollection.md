# Classe ObjectIdCollection

## Vue d'Ensemble
`ObjectIdCollection` est un conteneur spécialisé conçu pour contenir une liste de structures `ObjectId`. Il est extensivement utilisé dans l'API AutoCAD pour des opérations nécessitant plusieurs références d'objets, telles que l'effacement en masse, la gestion de transactions, et le passage de jeux de sélection aux méthodes API.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Count` | `int` | Obtient le nombre d'IDs dans la collection. |
| `this[int]` | `ObjectId` | Indexeur pour obtenir/définir l'ID à l'index spécifique. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Add(ObjectId)` | `int` | Ajoute un ID à la fin. Retourne constant 0. |
| `Remove(ObjectId)` | `void` | Supprime la première occurrence de l'ID. |
| `Contains(ObjectId)` | `bool` | Vérifie si l'ID existe dans la collection. |
| `Clear()` | `void` | Supprime tous les éléments. |
| `ToArray()` | `ObjectId[]` | Copie les éléments vers un nouveau tableau. |
| `CopyTo(ObjectId[], int)` | `void` | Copie les éléments vers un tableau existant. |

## Exemples de Code

### Exemple 1: Création et Remplissage
```csharp
ObjectIdCollection ids = new ObjectIdCollection();

// En supposant que vous avez des IDs d'objets
ids.Add(lineId);
ids.Add(circleId);
ids.Add(arcId);

Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nLa collection a {ids.Count} éléments.");
```

### Exemple 2: Utilisation avec Transaction (Effacement en Masse)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectIdCollection idsToErase = new ObjectIdCollection();
    
    // ... rassembler IDs ...
    idsToErase.Add(someId);
    
    foreach (ObjectId id in idsToErase)
    {
        DBObject obj = tr.GetObject(id, OpenMode.ForWrite);
        obj.Erase();
    }
    
    tr.Commit();
}
```

### Exemple 3: Convertir SelectionSet en Collection
```csharp
PromptSelectionResult selRes = ed.GetSelection();
if (selRes.Status == PromptStatus.OK)
{
    // SelectionSet.GetObjectIds retourne ObjectId[]
    ObjectId[] idArray = selRes.Value.GetObjectIds();
    
    // Créer collection depuis tableau
    ObjectIdCollection col = new ObjectIdCollection(idArray);
    
    ed.WriteMessage($"\nConverti {col.Count} éléments sélectionnés.");
}
```

### Exemple 4: Itération
```csharp
foreach (ObjectId id in myCollection)
{
    // Effectuer vérification
    if (id.IsNull) continue;
    
    // Traiter ID
}
```

## Meilleures Pratiques
1. **Libération Non Requise** : Contrairement à `DBObject`, `ObjectIdCollection` est un wrapper géré autour d'une liste simple et ne nécessite pas strictement de libération, mais implémente généralement `IDisposable` dans les wrappers COM. En .NET, c'est généralement sûr, mais les blocs `using` sont une bonne pratique si disponible.
2. **Performance** : Pour des listes massives (100k+), `List<ObjectId>` standard pourrait être légèrement plus rapide, mais les méthodes API nécessitent spécifiquement `ObjectIdCollection`. Utilisez la collection native lors de l'interaction avec les méthodes AutoCAD.

## Objets Associés
- [ObjectId](../BaseClasses/ObjectId.md) - Le type d'élément.
- [DBObjectCollection](DBObjectCollection.md) - Collection des objets eux-mêmes.

## Références
- [Référence ObjectIdCollection Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ObjectIdCollection)
