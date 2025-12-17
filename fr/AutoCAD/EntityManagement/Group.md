# Group

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `Group` représente une collection nommée d'entités qui peuvent être sélectionnées et manipulées comme une seule unité. Les groupes fournissent un moyen d'organiser les objets connexes sans créer un bloc.

**Concept Clé:** Les groupes sont comme des jeux de sélection qui persistent dans le dessin. Contrairement aux blocs, les entités groupées maintiennent leurs propriétés individuelles et peuvent être modifiées indépendamment.

## Hiérarchie de Classe

```
Object
  └─ RXObject
      └─ DBObject
          └─ Group
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom du groupe |
| `Description` | `string` | Obtient/définit la description du groupe |
| `Selectable` | `bool` | Obtient/définit si le groupe peut être sélectionné comme unité |
| `IsAnonymous` | `bool` | Vérifie si le groupe est anonyme (sans nom) |
| `NumEntities` | `int` | Obtient le nombre d'entités dans le groupe |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Append(ObjectId)` | `void` | Ajoute une entité au groupe |
| `Remove(ObjectId)` | `void` | Supprime une entité du groupe |
| `GetAllEntityIds()` | `ObjectId[]` | Obtient tous les IDs d'entités dans le groupe |
| `Has(ObjectId)` | `bool` | Vérifie si l'entité est dans le groupe |
| `Clear()` | `void` | Supprime toutes les entités du groupe |

## Exemples de Code

### Création d'un Groupe

```csharp
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;

// Sélectionner des entités
PromptSelectionResult psr = ed.GetSelection();
if (psr.Status != PromptStatus.OK) return;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
        OpenMode.ForWrite) as DBDictionary;
    
    // Créer un nouveau groupe
    Group group = new Group("Description du groupe", true);
    group.Name = "Mon_Groupe";
    
    // Ajouter au dictionnaire de groupes
    groupDict.SetAt("Mon_Groupe", group);
    tr.AddNewlyCreatedDBObject(group, true);
    
    // Ajouter des entités au groupe
    foreach (SelectedObject so in psr.Value)
    {
        group.Append(so.ObjectId);
    }
    
    tr.Commit();
    ed.WriteMessage($"\nGroupe créé avec {group.NumEntities} entités");
}
```

### Liste de Tous les Groupes

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
        OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\n=== Groupes ===");
    
    foreach (DBDictionaryEntry entry in groupDict)
    {
        Group group = tr.GetObject(entry.Value, OpenMode.ForRead) as Group;
        
        ed.WriteMessage($"\n\nNom : {group.Name}");
        ed.WriteMessage($"\n  Description : {group.Description}");
        ed.WriteMessage($"\n  Entités : {group.NumEntities}");
        ed.WriteMessage($"\n  Sélectionnable : {group.Selectable}");
    }
    
    tr.Commit();
}
```

### Ajout d'Entités à un Groupe Existant

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, 
        OpenMode.ForRead) as DBDictionary;
    
    Group group = tr.GetObject(groupDict.GetAt("Mon_Groupe"), 
        OpenMode.ForWrite) as Group;
    
    // Sélectionner des entités à ajouter
    PromptSelectionResult psr = ed.GetSelection();
    if (psr.Status == PromptStatus.OK)
    {
        foreach (SelectedObject so in psr.Value)
        {
            if (!group.Has(so.ObjectId))
            {
                group.Append(so.ObjectId);
            }
        }
    }
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Noms Uniques**: Utiliser des noms de groupe descriptifs et uniques
2. **Drapeau Sélectionnable**: Définir Selectable à true pour l'interaction utilisateur
3. **Description**: Fournir des descriptions claires
4. **Vérifier l'Appartenance**: Utiliser Has() avant de supprimer des entités
5. **Préserver les Entités**: La suppression du groupe ne supprime pas les entités
6. **Groupes Anonymes**: Utiliser pour le regroupement temporaire

## Classes Associées

- [DBDictionary](../Core/DBDictionary.md) - Contient les groupes
- ObjectIdCollection - Collection d'IDs d'entités
- SelectionSet - Sélection temporaire

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
