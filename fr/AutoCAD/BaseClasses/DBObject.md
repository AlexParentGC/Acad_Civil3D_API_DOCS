# Classe DBObject

## Vue d'Ensemble
La classe `DBObject` est la classe de base fondamentale pour tous les objets stockés dans une base de données AutoCAD. Elle fournit les fonctionnalités de base pour la persistance des objets, les transactions, les handles et la gestion du cycle de vie des objets. Tout objet dans la base de données de dessin — qu'il soit graphique (entités) ou non graphique (enregistrements de table de symboles, dictionnaires, etc.) — dérive de `DBObject`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          ├─ Entity (tous les objets graphiques)
          ├─ SymbolTableRecord (calques, blocs, etc.)
          ├─ DBDictionary
          ├─ XRecord
          ├─ SymbolTable
          └─ ... (tous les objets de base de données)
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ObjectId` | `ObjectId` | Obtient l'ObjectId de cet objet |
| `Handle` | `Handle` | Obtient le handle unique de cet objet |
| `Database` | `Database` | Obtient la base de données contenant cet objet |
| `IsErased` | `bool` | Obtient si l'objet a été effacé |
| `IsModified` | `bool` | Obtient si l'objet a été modifié |
| `IsNewObject` | `bool` | Obtient si c'est un objet nouvellement créé |
| `IsNotifying` | `bool` | Obtient si l'objet notifie les réacteurs |
| `IsReadEnabled` | `bool` | Obtient si l'objet est ouvert pour lecture |
| `IsWriteEnabled` | `bool` | Obtient si l'objet est ouvert pour écriture |
| `IsUndoing` | `bool` | Obtient si une opération d'annulation est en cours |
| `ExtensionDictionary` | `ObjectId` | Obtient l'ObjectId du dictionnaire d'extension |
| `OwnerId` | `ObjectId` | Obtient l'ObjectId de l'objet propriétaire |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Erase()` | `void` | Efface (suppression logicielle) l'objet |
| `Erase(bool)` | `void` | Efface ou restaure l'objet |
| `UpgradeOpen()` | `void` | Met à niveau l'objet de lecture à écriture |
| `DowngradeOpen()` | `void` | Rétrograde l'objet d'écriture à lecture |
| `Cancel()` | `void` | Annule les modifications apportées à l'objet |
| `CreateExtensionDictionary()` | `ObjectId` | Crée un dictionnaire d'extension |
| `GetRXClass()` | `RXClass` | Obtient les informations de classe d'exécution |
| `Dispose()` | `void` | Libère l'objet |

## Exemples de Code

### Exemple 1: Obtenir des Informations sur l'Objet
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage("\n=== Informations sur l'Objet ===");
    ed.WriteMessage($"\nType : {obj.GetType().Name}");
    ed.WriteMessage($"\nObjectId : {obj.ObjectId}");
    ed.WriteMessage($"\nHandle : {obj.Handle}");
    ed.WriteMessage($"\nBase de Données : {obj.Database.Filename}");
    ed.WriteMessage($"\nPropriétaire : {obj.OwnerId}");
    ed.WriteMessage($"\nEst Effacé : {obj.IsErased}");
    ed.WriteMessage($"\nEst Modifié : {obj.IsModified}");
    ed.WriteMessage($"\nEst Nouveau : {obj.IsNewObject}");
    
    tr.Commit();
}
```

### Exemple 2: Ouvrir des Objets pour Lecture et Écriture
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Ouvrir pour lecture
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage($"\nLecture activée : {obj.IsReadEnabled}");
    ed.WriteMessage($"\nÉcriture activée : {obj.IsWriteEnabled}");
    
    // Mettre à niveau vers écriture
    obj.UpgradeOpen();
    
    ed.WriteMessage($"\nAprès mise à niveau - Écriture activée : {obj.IsWriteEnabled}");
    
    // Faire des modifications...
    if (obj is Entity ent)
    {
        ent.ColorIndex = 1; // Changer en rouge
    }
    
    // Rétrograder vers lecture
    obj.DowngradeOpen();
    
    ed.WriteMessage($"\nAprès rétrogradation - Écriture activée : {obj.IsWriteEnabled}");
    
    tr.Commit();
}
```

### Exemple 3: Effacer et Restaurer des Objets
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForWrite);
    
    // Suppression logicielle (erase)
    obj.Erase();
    
    ed.WriteMessage($"\nObjet effacé : {obj.IsErased}");
    
    // Restaurer (unerase)
    obj.Erase(false);
    
    ed.WriteMessage($"\nObjet restauré : {!obj.IsErased}");
    
    tr.Commit();
}
```

### Exemple 4: Travailler avec des Dictionnaires d'Extension
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForWrite);
    
    // Vérifier si le dictionnaire d'extension existe
    if (obj.ExtensionDictionary == ObjectId.Null)
    {
        // Créer dictionnaire d'extension
        obj.CreateExtensionDictionary();
        ed.WriteMessage("\nDictionnaire d'extension créé");
    }
    
    // Accéder au dictionnaire d'extension
    DBDictionary extDict = tr.GetObject(obj.ExtensionDictionary, OpenMode.ForWrite) as DBDictionary;
    
    // Ajouter données personnalisées
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "DonnéePerso"),
        new TypedValue((int)DxfCode.Int32, 12345)
    );
    xRec.Data = rb;
    
    extDict.SetAt("MesDonnées", xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    ed.WriteMessage($"\nEntrées du dictionnaire d'extension : {extDict.Count}");
    
    tr.Commit();
}
```

### Exemple 5: Vérifier l'État de l'Objet
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage("\n=== État de l'Objet ===");
    ed.WriteMessage($"\nEst Lecture Activée : {obj.IsReadEnabled}");
    ed.WriteMessage($"\nEst Écriture Activée : {obj.IsWriteEnabled}");
    ed.WriteMessage($"\nEst Effacé : {obj.IsErased}");
    ed.WriteMessage($"\nEst Modifié : {obj.IsModified}");
    ed.WriteMessage($"\nEst Nouvel Objet : {obj.IsNewObject}");
    ed.WriteMessage($"\nEst Annulation : {obj.IsUndoing}");
    ed.WriteMessage($"\nEst Notification : {obj.IsNotifying}");
    
    tr.Commit();
}
```

### Exemple 6: Annuler les Modifications
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Sauvegarder couleur originale
    short originalColor = ent.ColorIndex;
    ed.WriteMessage($"\nCouleur originale : {originalColor}");
    
    // Faire des modifications
    ent.ColorIndex = 1; // Rouge
    ed.WriteMessage($"\nCouleur changée à : {ent.ColorIndex}");
    
    // Annuler les modifications
    ent.Cancel();
    
    ed.WriteMessage($"\nAprès annulation : {ent.ColorIndex}");
    
    // Ne pas valider - les modifications sont annulées
    tr.Abort();
}
```

### Exemple 7: Obtenir les Informations de Classe d'Exécution
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    RXClass rxClass = obj.GetRXClass();
    
    ed.WriteMessage("\n=== Info Classe Exécution ===");
    ed.WriteMessage($"\nNom Classe : {rxClass.Name}");
    ed.WriteMessage($"\nNom DXF : {rxClass.DxfName}");
    ed.WriteMessage($"\nNom App : {rxClass.AppName}");
    
    // Vérifier hiérarchie de classe
    RXClass parent = rxClass.MyParent;
    while (parent != null)
    {
        ed.WriteMessage($"\nParent : {parent.Name}");
        parent = parent.MyParent;
    }
    
    tr.Commit();
}
```

### Exemple 8: Itérer à Travers Tous les Objets de la Base de Données
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Itérer à travers tous les objets dans la base de données
    Dictionary<string, int> objectCounts = new Dictionary<string, int>();
    
    // Obtenir toutes les entités dans l'espace objet
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForRead) as BlockTableRecord;
    
    foreach (ObjectId objId in modelSpace)
    {
        DBObject obj = tr.GetObject(objId, OpenMode.ForRead);
        string typeName = obj.GetType().Name;
        
        if (objectCounts.ContainsKey(typeName))
            objectCounts[typeName]++;
        else
            objectCounts[typeName] = 1;
    }
    
    ed.WriteMessage("\n=== Compte Types d'Objets ===");
    foreach (var kvp in objectCounts.OrderByDescending(x => x.Value))
    {
        ed.WriteMessage($"\n{kvp.Key} : {kvp.Value}");
    }
    
    tr.Commit();
}
```

### Exemple 9: Comparer des Objets par Handle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj1 = tr.GetObject(objectId1, OpenMode.ForRead);
    DBObject obj2 = tr.GetObject(objectId2, OpenMode.ForRead);
    
    // Comparer par ObjectId
    bool sameById = obj1.ObjectId == obj2.ObjectId;
    
    // Comparer par Handle
    bool sameByHandle = obj1.Handle == obj2.Handle;
    
    ed.WriteMessage($"\nIdentique par ObjectId : {sameById}");
    ed.WriteMessage($"\nIdentique par Handle : {sameByHandle}");
    ed.WriteMessage($"\nObj1 Handle : {obj1.Handle}");
    ed.WriteMessage($"\nObj2 Handle : {obj2.Handle}");
    
    tr.Commit();
}
```

### Exemple 10: Trouver le Propriétaire de l'Objet
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage("\n=== Chaîne de Propriété de l'Objet ===");
    ed.WriteMessage($"\nObjet : {obj.GetType().Name} ({obj.Handle})");
    
    // Remonter la chaîne de propriété
    ObjectId currentOwnerId = obj.OwnerId;
    int level = 1;
    
    while (!currentOwnerId.IsNull)
    {
        DBObject owner = tr.GetObject(currentOwnerId, OpenMode.ForRead);
        ed.WriteMessage($"\n{new string(' ', level * 2)}Propriétaire {level} : {owner.GetType().Name} ({owner.Handle})");
        
        currentOwnerId = owner.OwnerId;
        level++;
        
        // Empêcher les boucles infinies
        if (level > 10) break;
    }
    
    tr.Commit();
}
```

## Cycle de Vie de l'Objet

### Création
1. Créer une nouvelle instance d'objet
2. Ajouter au conteneur (BlockTableRecord, SymbolTable, Dictionary, etc.)
3. Appeler `Transaction.AddNewlyCreatedDBObject()`
4. Valider la transaction

### Modification
1. Ouvrir l'objet avec `OpenMode.ForWrite`
2. Modifier les propriétés
3. Valider la transaction (ou appeler `Cancel()` pour annuler)

### Suppression
1. Ouvrir l'objet avec `OpenMode.ForWrite`
2. Appeler `Erase()` pour la suppression logicielle
3. Valider la transaction
4. Appeler `Erase(false)` pour restaurer si nécessaire

## Modes d'Ouverture

| Mode | Description | Usage |
|------|-------------|-------|
| `ForRead` | Accès lecture seule | Interrogation de propriétés, pas de modifications |
| `ForWrite` | Accès lecture et écriture | Modification de propriétés, effacement |
| `ForNotify` | Notification seulement | Notifications de réacteur |

## Meilleures Pratiques

1. **Toujours Utiliser des Transactions** : Ne jamais travailler avec des DBObjects en dehors des transactions
2. **Ouvrir avec le Mode Correct** : Utilisez `ForRead` quand c'est possible pour éviter le verrouillage
3. **Mettre à Niveau Si Nécessaire** : Utilisez `UpgradeOpen()` pour passer de lecture à écriture
4. **Libérer Correctement** : Laissez les transactions gérer la libération, ou utilisez des instructions `using`
5. **Vérifier IsErased** : Vérifiez que l'objet n'a pas été supprimé avant d'y accéder
6. **Gérer les Exceptions** : Enveloppez les opérations de base de données dans des blocs try-catch
7. **Utiliser des Handles pour la Persistance** : Les handles persistent entre les sessions, les ObjectIds non
8. **Dictionnaires d'Extension** : Utilisez pour le stockage de données personnalisées sur les objets
9. **Vérifier le Propriétaire** : Comprenez la propriété de l'objet avant d'effacer

## Objets Associés
- [Entity](Entity.md) - Classe dérivée pour les objets graphiques
- [DBDictionary](../Core/DBDictionary.md) - Classe dérivée pour les dictionnaires
- [XRecord](../Core/XRecord.md) - Classe dérivée pour les données personnalisées
- [Database](../Core/Database.md) - Contient tous les DBObjects

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
