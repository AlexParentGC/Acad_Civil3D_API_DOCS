# Classe ViewTable

## Vue d'Ensemble
La classe `ViewTable` est une table de symboles qui contient toutes les définitions de vues nommées dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ ViewTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si une vue existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId de la vue par nom (indexeur) |

## Exemples de Code

### Exemple 1: Lister Toutes les Vues Nommées
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ViewTable vt = tr.GetObject(db.ViewTableId, OpenMode.ForRead) as ViewTable;
    
    ed.WriteMessage("\nVues nommées dans le dessin :");
    
    foreach (ObjectId viewId in vt)
    {
        ViewTableRecord vtr = tr.GetObject(viewId, OpenMode.ForRead) as ViewTableRecord;
        
        ed.WriteMessage($"\n  {vtr.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient ViewTableId
- ViewTableRecord - Définition de vue nommée

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ViewTable)
