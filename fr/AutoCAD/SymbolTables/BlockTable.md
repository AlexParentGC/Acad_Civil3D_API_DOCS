# Classe BlockTable

## Vue d'Ensemble
La classe `BlockTable` est une table de symboles qui contient toutes les définitions de blocs dans un dessin AutoCAD, incluant l'Espace Objet et l'Espace Papier.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ BlockTable
```

## Propriétés Clés

Hérite de `SymbolTable` - agit comme une collection d'objets `BlockTableRecord`.

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Has(string)` | `bool` | Vérifie si un bloc existe par nom |
| `this[string]` | `ObjectId` | Obtient l'ObjectId du bloc par nom (indexeur) |

## Exemples de Code

### Exemple 1: Accéder à la Table des Blocs
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Accéder Espace Objet
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Accéder Espace Papier
    BlockTableRecord paperSpace = tr.GetObject(bt[BlockTableRecord.PaperSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    tr.Commit();
}
```

### Exemple 2: Lister Tous les Blocs
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    ed.WriteMessage("\nBlocs dans le dessin :");
    
    foreach (ObjectId btrId in bt)
    {
        BlockTableRecord btr = tr.GetObject(btrId, OpenMode.ForRead) as BlockTableRecord;
        
        // Sauter les blocs de présentation et les blocs anonymes
        if (!btr.IsLayout && !btr.IsAnonymous)
        {
            ed.WriteMessage($"\n  {btr.Name}");
        }
    }
    
    tr.Commit();
}
```

### Exemple 3: Vérifier si un Bloc Existe
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    string blockName = "MonBloc";
    
    if (bt.Has(blockName))
    {
        ObjectId blockId = bt[blockName];
        BlockTableRecord btr = tr.GetObject(blockId, OpenMode.ForRead) as BlockTableRecord;
        
        ed.WriteMessage($"\nBloc '{blockName}' trouvé");
    }
    else
    {
        ed.WriteMessage($"\nBloc '{blockName}' non trouvé");
    }
    
    tr.Commit();
}
```

### Exemple 4: Créer une Nouvelle Définition de Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForWrite) as BlockTable;
    
    // Créer un nouvel enregistrement de table de bloc
    BlockTableRecord btr = new BlockTableRecord();
    btr.Name = "MonNouveauBloc";
    
    // Ajouter un cercle au bloc
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 10);
    btr.AppendEntity(circle);
    
    // Ajouter bloc à la table
    bt.Add(btr);
    tr.AddNewlyCreatedDBObject(btr, true);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}
```

## Objets Associés
- [Database](../Core/Database.md) - Contient BlockTableId
- [BlockReference](../Entities/Complex/BlockReference.md) - Insertion de bloc
- BlockTableRecord - Définition de bloc (conteneur)

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_BlockTable)
