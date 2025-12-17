# Database Events

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `Database` expose de nombreux événements qui se déclenchent lorsque des objets de base de données sont créés, modifiés ou supprimés. Ces événements permettent des modèles de programmation réactive où votre code répond automatiquement aux changements dans le dessin.

**Concept Clé:** Les événements de base de données vous permettent de surveiller et de réagir aux changements dans la base de données de dessin sans interrogation. Ils sont essentiels pour maintenir l'intégrité des données, la synchronisation et l'implémentation de comportements personnalisés.

## Catégories d'Événements

### Événements d'Objet
Événements qui se déclenchent lorsque des objets sont ajoutés, modifiés ou effacés.

### Événements de Transaction  
Événements liés au cycle de vie des transactions.

### Événements de Sauvegarde/Chargement
Événements qui se déclenchent pendant les opérations de sauvegarde et de chargement de dessin.

### Événements de Modification
Événements pour suivre les modifications de la base de données.

## Événements Clés

| Événement | Description |
|-----------|-------------|
| `ObjectAppended` | Se déclenche lorsqu'un objet est ajouté à la base de données |
| `ObjectErased` | Se déclenche lorsqu'un objet est effacé |
| `ObjectModified` | Se déclenche lorsqu'un objet est modifié |
| `ObjectReappended` | Se déclenche lorsqu'un objet effacé est restauré |
| `ObjectUnappended` | Se déclenche lorsqu'un objet ajouté est supprimé |
| `ObjectOpenedForModify` | Se déclenche lorsqu'un objet est ouvert en écriture |
| `BeginSave` | Se déclenche avant la sauvegarde du dessin |
| `SaveComplete` | Se déclenche après la sauvegarde du dessin |
| `BeginDeepClone` | Se déclenche avant une opération de clonage profond |
| `BeginDeepCloneTranslation` | Se déclenche pendant la traduction de clonage profond |

## Modèles d'Utilisation Courants

### 1. Surveillance de la Création d'Objets

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;

public class DatabaseEventHandler
{
    private Database _db;
    
    public void RegisterEvents()
    {
        Document doc = Application.DocumentManager.MdiActiveDocument;
        _db = doc.Database;
        
        _db.ObjectAppended += OnObjectAppended;
    }
    
    public void UnregisterEvents()
    {
        if (_db != null)
        {
            _db.ObjectAppended -= OnObjectAppended;
        }
    }
    
    private void OnObjectAppended(object sender, ObjectEventArgs e)
    {
        Document doc = Application.DocumentManager.MdiActiveDocument;
        Editor ed = doc.Editor;
        
        using (Transaction tr = _db.TransactionManager.StartTransaction())
        {
            DBObject obj = tr.GetObject(e.DBObject.ObjectId, OpenMode.ForRead);
            
            ed.WriteMessage($"\n>>> Objet Ajouté : {obj.GetType().Name}");
            
            if (obj is Entity ent)
            {
                ed.WriteMessage($" sur le calque '{ent.Layer}'");
            }
            
            tr.Commit();
        }
    }
}
```

### 2. Suivi des Modifications d'Objets

```csharp
public class ModificationTracker
{
    private Database _db;
    private int _modificationCount = 0;
    
    public void Start()
    {
        _db = Application.DocumentManager.MdiActiveDocument.Database;
        _db.ObjectModified += OnObjectModified;
    }
    
    public void Stop()
    {
        if (_db != null)
        {
            _db.ObjectModified -= OnObjectModified;
            
            Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
            ed.WriteMessage($"\nTotal des modifications suivies : {_modificationCount}");
        }
    }
    
    private void OnObjectModified(object sender, ObjectEventArgs e)
    {
        _modificationCount++;
        
        using (Transaction tr = _db.TransactionManager.StartTransaction())
        {
            DBObject obj = tr.GetObject(e.DBObject.ObjectId, OpenMode.ForRead);
            
            Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
            ed.WriteMessage($"\n>>> Modifié : {obj.GetType().Name} (Total : {_modificationCount})");
            
            tr.Commit();
        }
    }
}
```

### 3. Surveillance des Opérations d'Effacement/Restauration

```csharp
public class EraseMonitor
{
    private Database _db;
    
    public void RegisterEvents()
    {
        _db = Application.DocumentManager.MdiActiveDocument.Database;
        
        _db.ObjectErased += OnObjectErased;
        _db.ObjectReappended += OnObjectReappended;
    }
    
    private void OnObjectErased(object sender, ObjectErasedEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        
        if (e.Erased)
        {
            ed.WriteMessage($"\n>>> Objet EFFACÉ : {e.DBObject.GetType().Name}");
        }
        else
        {
            ed.WriteMessage($"\n>>> Objet RESTAURÉ : {e.DBObject.GetType().Name}");
        }
    }
    
    private void OnObjectReappended(object sender, ObjectEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Objet RÉAJOUTÉ : {e.DBObject.GetType().Name}");
    }
}
```

### 4. Gestion des Événements de Sauvegarde

```csharp
public class SaveEventHandler
{
    private Database _db;
    
    public void RegisterEvents()
    {
        _db = Application.DocumentManager.MdiActiveDocument.Database;
        
        _db.BeginSave += OnBeginSave;
        _db.SaveComplete += OnSaveComplete;
    }
    
    private void OnBeginSave(object sender, DatabaseIOEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Début de Sauvegarde : {e.FileName}");
        
        // Effectuer des opérations pré-sauvegarde
        // par ex., valider les données, mettre à jour les métadonnées, etc.
    }
    
    private void OnSaveComplete(object sender, DatabaseIOEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Sauvegarde Terminée : {e.FileName}");
        
        // Effectuer des opérations post-sauvegarde
        // par ex., sauvegarde, journalisation, etc.
    }
}
```

## Meilleures Pratiques

1. **Toujours désenregistrer**: Supprimer les gestionnaires d'événements pour éviter les fuites mémoire
2. **Utiliser des transactions**: Toujours utiliser des transactions lors de l'accès aux objets dans les gestionnaires d'événements
3. **Éviter la récursion**: Faire attention à ne pas déclencher le même événement dans son gestionnaire
4. **Garder les gestionnaires rapides**: Les gestionnaires d'événements doivent s'exécuter rapidement
5. **Gérer les exceptions**: Envelopper le code du gestionnaire d'événements dans des blocs try-catch
6. **Spécifique au document**: Enregistrer les événements par document, pas globalement

## Modèles Communs

### Modèle 1: Enregistrer/Désenregistrer
```csharp
// Enregistrer
_db.ObjectAppended += OnObjectAppended;

// Désenregistrer (CRITIQUE!)
_db.ObjectAppended -= OnObjectAppended;
```

### Modèle 2: Accès Sécurisé aux Objets
```csharp
private void OnEvent(object sender, ObjectEventArgs e)
{
    using (Transaction tr = _db.TransactionManager.StartTransaction())
    {
        DBObject obj = tr.GetObject(e.DBObject.ObjectId, OpenMode.ForRead);
        // Traiter l'objet
        tr.Commit();
    }
}
```

### Modèle 3: Traitement Conditionnel
```csharp
private void OnObjectAppended(object sender, ObjectEventArgs e)
{
    if (e.DBObject is Circle)
    {
        // Traiter uniquement les cercles
    }
}
```

## Pièges Courants

❌ **Oublier de désenregistrer**: Cause des fuites mémoire  
❌ **Modifier dans le gestionnaire d'événements**: Peut causer de la récursion  
❌ **Ne pas utiliser de transactions**: Cause des violations d'accès  
❌ **Gestionnaires d'événements lents**: Dégrade les performances  
❌ **Gestionnaires d'événements globaux**: Doivent être spécifiques au document

## Classes Associées

- [Database](../Core/Database.md) - Expose ces événements
- ObjectEventArgs - Classe d'arguments d'événement
- [Transaction](../Transactions/Transaction.md) - Requis pour l'accès aux objets
- [Document](../Core/Document.md) - Événements au niveau du document

## Voir Aussi

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-DatabaseEvents)
