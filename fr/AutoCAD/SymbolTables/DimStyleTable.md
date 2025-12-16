# Classe DimStyleTable

## Vue d'Ensemble
La classe `DimStyleTable` est une table de symboles qui contient toutes les définitions de styles de cote dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ DimStyleTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un style de cote existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId du style de cote par nom (indexeur) |

## Exemples de Code

### Exemple 1: Lister Tous les Styles de Cote
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
    
    ed.WriteMessage("\nStyles de cote dans le dessin :");
    
    foreach (ObjectId styleId in dst)
    {
        DimStyleTableRecord dstr = tr.GetObject(styleId, OpenMode.ForRead) as DimStyleTableRecord;
        
        ed.WriteMessage($"\n  {dstr.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient DimStyleTableId
- [Dimension](../Entities/Annotations/Dimension.md) - Utilise les styles de cote
- DimStyleTableRecord - Définition de style de cote

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DimStyleTable)
