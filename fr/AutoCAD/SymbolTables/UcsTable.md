# Classe UcsTable

## Vue d'Ensemble
La classe `UcsTable` est une table de symboles qui contient toutes les définitions de Système de Coordonnées Utilisateur (SCU) dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ UcsTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un SCU existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId du SCU par nom (indexeur) |

## Exemples de Code

### Exemple 1: Lister Toutes les Définitions de SCU
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    UcsTable ut = tr.GetObject(db.UcsTableId, OpenMode.ForRead) as UcsTable;
    
    ed.WriteMessage("\nDéfinitions SCU dans le dessin :");
    
    foreach (ObjectId ucsId in ut)
    {
        UcsTableRecord utr = tr.GetObject(ucsId, OpenMode.ForRead) as UcsTableRecord;
        
        ed.WriteMessage($"\n  {utr.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient UcsTableId
- UcsTableRecord - Définition de SCU

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_UcsTable)
