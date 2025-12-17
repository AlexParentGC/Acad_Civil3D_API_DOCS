# Transaction

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `Transaction` est le mécanisme fondamental pour accéder et modifier en toute sécurité les objets de base de données AutoCAD. Les transactions garantissent l'intégrité des données en fournissant des opérations atomiques - soit tous les changements réussissent, soit tous sont annulés.

**Concept Clé:** Dans l'API .NET AutoCAD, vous devez ouvrir les objets de base de données dans une transaction pour les lire ou les modifier. Cela empêche la corruption et assure la sécurité des threads.

## Hiérarchie de Classe

```
Object
  └─ RXObject
      └─ DisposableWrapper
          └─ Transaction
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `AutoDelete` | `bool` | Obtient/définit si la transaction se supprime automatiquement après commit/abort |
| `TransactionManager` | `TransactionManager` | Obtient le gestionnaire de transactions qui a créé cette transaction |

## Méthodes Clés

### Cycle de Vie de la Transaction

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Commit()` | `void` | Valide tous les changements effectués pendant la transaction |
| `Abort()` | `void` | Annule tous les changements effectués pendant la transaction |
| `Dispose()` | `void` | Dispose la transaction (appelle Abort si non validée) |

### Accès aux Objets

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetObject(ObjectId, OpenMode)` | `DBObject` | Ouvre un objet de base de données en lecture ou écriture |
| `GetObject(ObjectId, OpenMode, bool)` | `DBObject` | Ouvre un objet avec option d'ouvrir les objets effacés |
| `AddNewlyCreatedDBObject(DBObject, bool)` | `void` | Ajoute un objet nouvellement créé à la transaction |

## Modèles d'Utilisation Courants

### 1. Modèle de Transaction de Base

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("BASICTRANS")]
public void BasicTransactionExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        try
        {
            // Ouvrir l'espace modèle en écriture
            BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
            BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
                OpenMode.ForWrite) as BlockTableRecord;
            
            // Créer un cercle
            Circle circle = new Circle(new Point3d(0, 0, 0), Vector3d.ZAxis, 10.0);
            
            // Ajouter à la base de données
            btr.AppendEntity(circle);
            tr.AddNewlyCreatedDBObject(circle, true);
            
            // Valider les changements
            tr.Commit();
        }
        catch (System.Exception ex)
        {
            doc.Editor.WriteMessage($"\nErreur : {ex.Message}");
            // La transaction s'annulera automatiquement dans Dispose si non validée
        }
    }
}
```

### 2. Lecture des Propriétés d'Objets

```csharp
[CommandMethod("READLAYERS")]
public void ReadLayersExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Ouvrir la table des calques en lecture
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        
        ed.WriteMessage("\n=== Calques dans le Dessin ===");
        foreach (ObjectId layerId in lt)
        {
            LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
            ed.WriteMessage($"\nCalque : {ltr.Name}, Couleur : {ltr.Color.ColorIndex}");
        }
        
        tr.Commit(); // Bonne pratique même pour les opérations en lecture seule
    }
}
```

### 3. Modification d'Objets Existants

```csharp
[CommandMethod("MODIFYENTITY")]
public void ModifyEntityExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Demander à l'utilisateur de sélectionner une entité
    PromptEntityOptions peo = new PromptEntityOptions("\nSélectionner un cercle à modifier : ");
    peo.SetRejectMessage("\nDoit être un cercle.");
    peo.AddAllowedClass(typeof(Circle), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Ouvrir le cercle en écriture
        Circle circle = tr.GetObject(per.ObjectId, OpenMode.ForWrite) as Circle;
        
        // Modifier les propriétés
        circle.Radius = circle.Radius * 2.0;
        circle.ColorIndex = 1; // Rouge
        
        ed.WriteMessage($"\nRayon du cercle doublé à {circle.Radius}");
        
        tr.Commit();
    }
}
```

## Meilleures Pratiques

1. **Toujours utiliser l'instruction `using`**: Assure que la transaction est disposée même si des exceptions se produisent
2. **Valider explicitement**: Appeler `Commit()` lorsque toutes les opérations réussissent
3. **Laisser Dispose gérer l'annulation**: Ne pas appeler `Abort()` explicitement; `Dispose()` annulera si non validé
4. **Ouvrir les objets avec permissions minimales**: Utiliser `OpenMode.ForRead` lorsque possible
5. **Minimiser la portée de la transaction**: Garder les transactions courtes pour réduire le verrouillage
6. **Gérer les exceptions**: Envelopper le code de transaction dans des blocs try-catch
7. **Ne pas franchir les limites de document**: Chaque document a son propre gestionnaire de transactions

## Erreurs Courantes

| Erreur | Cause | Solution |
|--------|-------|----------|
| `eNoActiveTransactions` | Accès à l'objet sans transaction | Démarrer une transaction d'abord |
| `eOnLockedLayer` | Modification d'objet sur calque verrouillé | Déverrouiller le calque ou ouvrir en lecture seule |
| `eWasErased` | Accès à un objet effacé | Utiliser `GetObject` avec `openErased: true` |
| `eNotOpenForWrite` | Modification d'objet en lecture seule | Ouvrir avec `OpenMode.ForWrite` |

## Classes Associées

- [TransactionManager](TransactionManager.md) - Gère le cycle de vie des transactions
- [OpenCloseTransaction](OpenCloseTransaction.md) - Modèle de transaction simplifié
- [DBObject](../BaseClasses/DBObject.md) - Classe de base pour tous les objets de base de données
- [Database](../Core/Database.md) - Base de données de dessin

## Voir Aussi

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-C0869B95-08F0-4C3C-8787-862EAEF1DB3F)
