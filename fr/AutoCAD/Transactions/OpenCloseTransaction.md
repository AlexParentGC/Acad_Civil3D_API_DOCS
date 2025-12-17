# OpenCloseTransaction

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `OpenCloseTransaction` fournit un modèle de transaction simplifié qui ouvre et ferme automatiquement les objets. Elle est plus simple que la classe Transaction standard mais offre moins de contrôle.

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Start()` | `void` | Démarre la transaction |
| `Commit()` | `void` | Valide la transaction |
| `Abort()` | `void` | Annule la transaction |
| `GetObject(ObjectId, OpenMode)` | `DBObject` | Obtient un objet |
| `AddNewlyCreatedDBObject(DBObject, bool)` | `void` | Ajoute un nouvel objet |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.ApplicationServices;

Document doc = Application.DocumentManager.MdiActiveDocument;
Database db = doc.Database;

using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
{
    BlockTable bt = (BlockTable)tr.GetObject(db.BlockTableId, OpenMode.ForRead);
    BlockTableRecord btr = (BlockTableRecord)tr.GetObject(
        bt[BlockTableRecord.ModelSpace], OpenMode.ForWrite);
    
    Line line = new Line(new Point3d(0, 0, 0), new Point3d(100, 100, 0));
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    tr.Commit();
}
```

## Différences avec Transaction

- **Simplicité**: Plus simple à utiliser
- **Performance**: Légèrement moins performant
- **Contrôle**: Moins de contrôle sur le cycle de vie des objets
- **Utilisation**: Bon pour les opérations simples

## Meilleures Pratiques

1. **Utiliser pour les opérations simples**: Idéal pour les modifications basiques
2. **Transaction pour les opérations complexes**: Utiliser Transaction standard pour plus de contrôle
3. **Toujours utiliser `using`**: Assure la disposition appropriée

## Classes Associées

- [Transaction](Transaction.md) - Transaction standard
- [TransactionManager](TransactionManager.md) - Gestionnaire de transactions
- [Database](../Core/Database.md) - Base de données

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
