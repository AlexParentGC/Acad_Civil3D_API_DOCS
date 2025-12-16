# Classe LayerTable

## Vue d'Ensemble
La classe `LayerTable` est une table de symboles qui contient toutes les définitions de calques dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ LayerTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un calque existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId du calque par nom (indexeur) |

## Exemples de Code

### Exemple 1: Lister Tous les Calques
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    ed.WriteMessage("\nCalques dans le dessin :");
    
    foreach (ObjectId layerId in lt)
    {
        LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
        
        ed.WriteMessage($"\n  {ltr.Name}");
        ed.WriteMessage($" - Couleur : {ltr.Color.ColorIndex}");
        ed.WriteMessage($" - {(ltr.IsFrozen ? "Gelé" : "Libéré")}");
        ed.WriteMessage($" - {(ltr.IsOff ? "Désactivé" : "Activé")}");
        ed.WriteMessage($" - {(ltr.IsLocked ? "Verrouillé" : "Déverrouillé")}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Créer un Nouveau Calque
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForWrite) as LayerTable;
    
    if (!lt.Has("MonCalque"))
    {
        LayerTableRecord ltr = new LayerTableRecord();
        ltr.Name = "MonCalque";
        ltr.Color = Color.FromColorIndex(ColorMethod.ByAci, 1); // Rouge
        
        lt.Add(ltr);
        tr.AddNewlyCreatedDBObject(ltr, true);
        
        ed.WriteMessage("\nCalque 'MonCalque' créé");
    }
    
    tr.Commit();
}
```

### Exemple 3: Modifier les Propriétés de Calque
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    if (lt.Has("MonCalque"))
    {
        LayerTableRecord ltr = tr.GetObject(lt["MonCalque"], OpenMode.ForWrite) as LayerTableRecord;
        
        ltr.Color = Color.FromColorIndex(ColorMethod.ByAci, 3); // Vert
        ltr.IsFrozen = false;
        ltr.IsOff = false;
        ltr.IsLocked = false;
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient LayerTableId
- [Entity](../BaseClasses/Entity.md) - Les entités ont une propriété Layer
- LayerTableRecord - Définition de calque

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_LayerTable)
