# Classe RegAppTable

## Vue d'Ensemble
La classe `RegAppTable` est une table de symboles qui contient tous les noms d'applications enregistrées dans un dessin AutoCAD. Les applications doivent s'enregistrer avant d'attacher des XData (données d'entité étendues) aux objets.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ RegAppTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si une application est enregistrée |
| `this[string]` | `ObjectId` | Obtient l'ObjectId de l'app enregistrée par nom (indexeur) |

## Exemples de Code

### Exemple 1: Enregistrer une Application pour XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    string appName = "MonApp";
    
    if (!rat.Has(appName))
    {
        rat.UpgradeOpen();
        
        RegAppTableRecord ratr = new RegAppTableRecord();
        ratr.Name = appName;
        
        rat.Add(ratr);
        tr.AddNewlyCreatedDBObject(ratr, true);
        
        ed.WriteMessage($"\nApplication enregistrée '{appName}'");
    }
    
    tr.Commit();
}
```

### Exemple 2: Lister les Applications Enregistrées
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    ed.WriteMessage("\nApplications enregistrées :");
    
    foreach (ObjectId appId in rat)
    {
        RegAppTableRecord ratr = tr.GetObject(appId, OpenMode.ForRead) as RegAppTableRecord;
        
        ed.WriteMessage($"\n  {ratr.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient RegAppTableId
- RegAppTableRecord - Application enregistrée
- XData - Données d'entité étendues

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_RegAppTable)
