# Classe SymbolTable

## Vue d'Ensemble
`SymbolTable` est la classe de base abstraite pour toutes les tables de symboles dans la base de données AutoCAD, telles que `BlockTable`, `LayerTable`, `LinetypeTable`, etc. Elle agit comme un dictionnaire spécialisé qui mappe des noms de chaînes (clés) aux `ObjectId`s des `SymbolTableRecord`s.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              ├─ BlockTable
              ├─ LayerTable
              ├─ LinetypeTable
              ├─ TextStyleTable
              ├─ DimStyleTable
              ├─ RegAppTable
              ├─ UcsTable
              ├─ ViewTable
              └─ ViewportTable
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `this[string]` | `ObjectId` | Indexeur pour obtenir l'ID d'enregistrement par nom. |
| `Count` | `int` | Nombre d'enregistrements dans la table. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un enregistrement avec le nom donné existe. |
| `Has(ObjectId)` | `bool` | Vérifie si un enregistrement avec l'ID donné existe dans cette table. |
| `Add(SymbolTableRecord)` | `ObjectId` | Ajoute un nouvel enregistrement à la table. |
| `GetEnumerator()` | `SymbolTableEnumerator` | Retourne un énumérateur pour les ObjectIds. |

## Exemples de Code

### Exemple 1: Vérifier l'Existence d'un Enregistrement
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Ouvrir une table dérivée (ex : LayerTable) comme SymbolTable
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as SymbolTable;
    
    if (table.Has("MyLayer"))
    {
        // Le calque existe
    }
    
    tr.Commit();
}
```

### Exemple 2: Ajouter un Nouvel Enregistrement
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // 1. Ouvrir la Table pour Écriture
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForWrite) as SymbolTable;
    
    // 2. Créer l'Enregistrement (Type dérivé)
    LayerTableRecord newRec = new LayerTableRecord();
    newRec.Name = "NewLayer";
    
    // 3. Ajouter à la Table
    // Add() retourne le nouvel ObjectId
    ObjectId newId = table.Add(newRec);
    
    // 4. Ajouter à la Transaction
    tr.AddNewlyCreatedDBObject(newRec, true);
    
    tr.Commit();
}
```

### Exemple 3: Itérer une SymbolTable
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as SymbolTable;
    
    // Énumérer les ObjectIds
    foreach (ObjectId id in table)
    {
        // Ouvrir chaque enregistrement
        SymbolTableRecord rec = tr.GetObject(id, OpenMode.ForRead) as SymbolTableRecord;
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nTrouvé : {rec.Name}");
    }
    
    tr.Commit();
}
```

### Exemple 4: Utiliser l'Indexeur
```csharp
ObjectId id = table["MyLayer"];
// Note : Lance KeyNotFoundException si "MyLayer" n'existe pas.
// Utilisez Has() d'abord si vous n'êtes pas sûr.
```

### Exemple 5: Sensibilité à la Casse
```csharp
// Les clés de table de symboles sont généralement insensibles à la casse dans AutoCAD
if (table.Has("mylayer") && table.Has("MYLAYER"))
{
    // Ceux-ci réfèrent au même enregistrement
}
```

### Exemple 6: Supprimer des Enregistrements
```csharp
// SymbolTable n'a PAS de méthode Remove() directement.
// Vous devez ouvrir l'Enregistrement lui-même et appeler Erase().
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as SymbolTable;
    
    if (table.Has("LayerToDelete"))
    {
        ObjectId id = table["LayerToDelete"];
        DBObject rec = tr.GetObject(id, OpenMode.ForWrite);
        rec.Erase(); // Cela le supprime de la table
    }
    
    tr.Commit();
}
```

### Exemple 7: Requête LINQ sur Table
```csharp
// Puisque SymbolTable implémente IEnumerable, utilisez Cast<ObjectId>()
var allIds = table.Cast<ObjectId>().ToList();
```

### Exemple 8: Vérifier la Propriété
```csharp
// Tout enregistrement ajouté à cette table aura cette table comme OwnerID
if (record.OwnerId == table.ObjectId)
{
    // L'enregistrement appartient à cette table
}
```

## Meilleures Pratiques
1. **Toujours utiliser des Transactions** : Utilisez `AddNewlyCreatedDBObject` immédiatement après `table.Add()`.
2. **Vérifier `Has()`** : Les vérifications sont peu coûteuses ; les exceptions des indexeurs invalides sont coûteuses.
3. **Erase vs Remove** : Rappelez-vous qu'il n'y a pas de méthode `Remove()` sur la table. Vous `Erase()` l'enregistrement.
4. **Utilisation de la Classe de Base** : Utilisez la référence `SymbolTable` lors de l'écriture de code générique qui gère n'importe quelle table (Calques, Types de Ligne, etc.) de manière identique.

## Objets Associés
- [SymbolTableRecord](SymbolTableRecord.md) - Les éléments contenus dans la table.
- [BlockTable](../SymbolTables/BlockTable.md) - Implémentation spécifique.
- [LayerTable](../SymbolTables/LayerTable.md) - Implémentation spécifique.

## Références
- [Référence SymbolTable Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_SymbolTable)
