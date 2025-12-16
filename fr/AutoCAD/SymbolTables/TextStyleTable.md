# Classe TextStyleTable

## Vue d'Ensemble
La classe `TextStyleTable` est une table de symboles qui contient toutes les définitions de styles de texte dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ TextStyleTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un style de texte existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId du style de texte par nom (indexeur) |

## Exemples de Code

### Exemple 1: Lister Tous les Styles de Texte
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForRead) as TextStyleTable;
    
    ed.WriteMessage("\nStyles de texte dans le dessin :");
    
    foreach (ObjectId styleId in tst)
    {
        TextStyleTableRecord tstr = tr.GetObject(styleId, OpenMode.ForRead) as TextStyleTableRecord;
        
        ed.WriteMessage($"\n  {tstr.Name}");
        ed.WriteMessage($" - Fonte : {tstr.FileName}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Créer un Nouveau Style de Texte
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForWrite) as TextStyleTable;
    
    if (!tst.Has("MonStyle"))
    {
        TextStyleTableRecord tstr = new TextStyleTableRecord();
        tstr.Name = "MonStyle";
        tstr.FileName = "arial.ttf";
        tstr.TextSize = 0.0; // Hauteur variable
        
        tst.Add(tstr);
        tr.AddNewlyCreatedDBObject(tstr, true);
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient TextStyleTableId
- [DBText](../Entities/Text/DBText.md) - Utilise les styles de texte
- [MText](../Entities/Text/MText.md) - Utilise les styles de texte
- TextStyleTableRecord - Définition de style de texte

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_TextStyleTable)
