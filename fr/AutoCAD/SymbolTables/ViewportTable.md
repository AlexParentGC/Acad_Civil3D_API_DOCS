# Classe ViewportTable

## Vue d'Ensemble
La classe `ViewportTable` est une table de symboles qui contient les configurations de fenêtres (viewports) dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ ViewportTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si une config de fenêtre existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId de la fenêtre par nom (indexeur) |

## Exemples de Code

### Exemple 1: Accéder à la Table des Fenêtres
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ViewportTable vpt = tr.GetObject(db.ViewportTableId, OpenMode.ForRead) as ViewportTable;
    
    ed.WriteMessage("\nConfigurations de fenêtres :");
    
    foreach (ObjectId vpId in vpt)
    {
        ViewportTableRecord vptr = tr.GetObject(vpId, OpenMode.ForRead) as ViewportTableRecord;
        
        ed.WriteMessage($"\n  {vptr.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient ViewportTableId
- ViewportTableRecord - Configuration de fenêtre

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ViewportTable)
