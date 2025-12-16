# Classe BlockReference

## Vue d'Ensemble
La classe `BlockReference` représente une instance (insertion) d'une définition de bloc dans AutoCAD. Elle est utilisée pour placer des copies de définitions de bloc avec une position, une échelle et une rotation spécifiques.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ BlockReference
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `BlockTableRecord` | `ObjectId` | Obtient/définit l'ObjectId de la définition de bloc |
| `Position` | `Point3d` | Obtient/définit le point d'insertion |
| `Rotation` | `double` | Obtient/définit l'angle de rotation (radians) |
| `ScaleFactors` | `Scale3d` | Obtient/définit les facteurs d'échelle X, Y, Z |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |
| `AttributeCollection` | `AttributeCollection` | Obtient la collection d'attributs |
| `IsDynamicBlock` | `bool` | Obtient si c'est un bloc dynamique |
| `DynamicBlockTableRecord` | `ObjectId` | Obtient la définition de bloc dynamique |
| `BlockTransform` | `Matrix3d` | Obtient la matrice de transformation |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetBlockReferenceIds(bool, bool)` | `ObjectIdCollection` | Obtient toutes les références à ce bloc |
| `ExplodeToOwnerSpace()` | `void` | Explose la référence de bloc |
| `ResetBlock()` | `void` | Réinitialise le bloc dynamique à l'état par défaut |

## Exemples de Code

### Exemple 1: Insérer un Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Vérifier si le bloc existe
    if (bt.Has("MonBloc"))
    {
        BlockReference blockRef = new BlockReference(new Point3d(100, 100, 0), bt["MonBloc"]);
        
        // Définir les propriétés
        blockRef.Rotation = Math.PI / 4; // 45 degrés
        blockRef.ScaleFactors = new Scale3d(2.0, 2.0, 1.0); // Échelle 2x en X et Y
        
        btr.AppendEntity(blockRef);
        tr.AddNewlyCreatedDBObject(blockRef, true);
    }
    
    tr.Commit();
}
```

### Exemple 2: Travailler avec des Attributs de Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    if (bt.Has("Cartouche"))
    {
        BlockReference blockRef = new BlockReference(new Point3d(0, 0, 0), bt["Cartouche"]);
        btr.AppendEntity(blockRef);
        tr.AddNewlyCreatedDBObject(blockRef, true);
        
        // Obtenir la définition de bloc pour trouver les définitions d'attributs
        BlockTableRecord blockDef = tr.GetObject(bt["Cartouche"], OpenMode.ForRead) as BlockTableRecord;
        
        // Ajouter des attributs
        foreach (ObjectId objId in blockDef)
        {
            DBObject obj = tr.GetObject(objId, OpenMode.ForRead);
            
            if (obj is AttributeDefinition attDef)
            {
                AttributeReference attRef = new AttributeReference();
                attRef.SetAttributeFromBlock(attDef, blockRef.BlockTransform);
                attRef.TextString = "Valeur pour " + attDef.Tag;
                
                blockRef.AttributeCollection.AppendAttribute(attRef);
                tr.AddNewlyCreatedDBObject(attRef, true);
            }
        }
    }
    
    tr.Commit();
}
```

### Exemple 3: Lire des Attributs de Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockReference blockRef = tr.GetObject(blockRefId, OpenMode.ForRead) as BlockReference;
    
    foreach (ObjectId attId in blockRef.AttributeCollection)
    {
        AttributeReference attRef = tr.GetObject(attId, OpenMode.ForRead) as AttributeReference;
        
        ed.WriteMessage($"\n{attRef.Tag}: {attRef.TextString}");
    }
    
    tr.Commit();
}
```

### Exemple 4: Trouver Toutes les Instances d'un Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    if (bt.Has("MonBloc"))
    {
        BlockTableRecord blockDef = tr.GetObject(bt["MonBloc"], OpenMode.ForRead) as BlockTableRecord;
        
        // Obtenir toutes les références à ce bloc
        ObjectIdCollection refIds = blockDef.GetBlockReferenceIds(true, true);
        
        ed.WriteMessage($"\nTrouvé {refIds.Count} instances de 'MonBloc'");
        
        foreach (ObjectId refId in refIds)
        {
            BlockReference blockRef = tr.GetObject(refId, OpenMode.ForRead) as BlockReference;
            ed.WriteMessage($"\n  Position: {blockRef.Position}");
        }
    }
    
    tr.Commit();
}
```

## Objets Associés
- [BlockTable](../../SymbolTables/BlockTable.md) - Collection de définitions de bloc
- [BlockTableRecord](../../SymbolTables/BlockTable.md) - Définition de bloc
- [AttributeReference](AttributeReference.md) - Instance d'attribut de bloc
- [Entity](../../BaseClasses/Entity.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_BlockReference)
