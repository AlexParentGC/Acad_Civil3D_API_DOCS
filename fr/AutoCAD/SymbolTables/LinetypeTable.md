# Classe LinetypeTable

## Vue d'Ensemble
La classe `LinetypeTable` est une table de symboles qui contient toutes les définitions de types de ligne dans un dessin AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ LinetypeTable
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un type de ligne existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId du type de ligne par nom (indexeur) |

## Exemples de Code

### Exemple 1: Lister Tous les Types de Ligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
    
    ed.WriteMessage("\nTypes de ligne dans le dessin :");
    
    foreach (ObjectId linetypeId in ltt)
    {
        LinetypeTableRecord lttr = tr.GetObject(linetypeId, OpenMode.ForRead) as LinetypeTableRecord;
        
        ed.WriteMessage($"\n  {lttr.Name}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Vérifier si un Type de Ligne Existe
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
    
    if (ltt.Has("DASHED"))
    {
        ed.WriteMessage("\nLe type de ligne DASHED est disponible");
    }
    else
    {
        ed.WriteMessage("\nType de ligne DASHED non trouvé - peut nécessiter de charger acad.lin");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient LinetypeTableId
- [Entity](../BaseClasses/Entity.md) - Les entités ont une propriété Linetype
- LinetypeTableRecord - Définition de type de ligne

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_LinetypeTable)
