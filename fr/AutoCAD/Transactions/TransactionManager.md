# TransactionManager

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `TransactionManager` gère le cycle de vie des transactions pour une base de données AutoCAD. Elle fournit des méthodes pour créer, gérer et coordonner les transactions.

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `TopTransaction` | `Transaction` | Obtient la transaction de niveau supérieur |
| `NumberOfActiveTransactions` | `int` | Obtient le nombre de transactions actives |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `StartTransaction()` | `Transaction` | Démarre une nouvelle transaction |
| `AddNewlyCreatedDBObject(DBObject, bool)` | `void` | Ajoute un nouvel objet à la transaction active |
| `GetObject(ObjectId, OpenMode)` | `DBObject` | Obtient un objet dans la transaction active |
| `GetObject(ObjectId, OpenMode, bool)` | `DBObject` | Obtient un objet avec option d'ouvrir les objets effacés |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.ApplicationServices;

Document doc = Application.DocumentManager.MdiActiveDocument;
Database db = doc.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForWrite) as BlockTableRecord;
    
    Circle circle = new Circle(new Point3d(0, 0, 0), Vector3d.ZAxis, 10.0);
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Une transaction à la fois**: Éviter les transactions imbriquées sauf si nécessaire
2. **Toujours valider ou annuler**: Ne jamais laisser une transaction ouverte
3. **Utiliser `using`**: Assure la disposition appropriée
4. **Vérifier NumberOfActiveTransactions**: Pour le débogage

## Classes Associées

- [Transaction](Transaction.md) - Classe de transaction
- [Database](../Core/Database.md) - Base de données de dessin
- [DBObject](../BaseClasses/DBObject.md) - Objets de base de données

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
