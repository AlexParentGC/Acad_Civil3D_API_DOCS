# AttributeDefinition

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `AttributeDefinition` définit un modèle pour les attributs dans une définition de bloc. Elle spécifie l'étiquette, l'invite, la valeur par défaut et les propriétés pour les attributs qui seront créés lorsque le bloc est inséré.

**Concept Clé:** Les AttributeDefinitions existent dans les définitions de blocs et servent de modèles. Lorsqu'un bloc est inséré, des AttributeReferences sont créées basées sur ces définitions.

## Hiérarchie de Classe

```
Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ DBText
                  └─ AttributeDefinition
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Tag` | `string` | Obtient/définit l'étiquette d'attribut (identifiant) |
| `Prompt` | `string` | Obtient/définit l'invite affichée lors de l'insertion du bloc |
| `TextString` | `string` | Obtient/définit la valeur par défaut |
| `Invisible` | `bool` | Obtient/définit si l'attribut est invisible |
| `Constant` | `bool` | Obtient/définit si la valeur de l'attribut est constante |
| `Verifiable` | `bool` | Obtient/définit s'il faut vérifier la valeur lors de l'insertion |
| `Preset` | `bool` | Obtient/définit si l'attribut est prédéfini |
| `LockPositionInBlock` | `bool` | Obtient/définit si la position est verrouillée dans le bloc |

## Exemples de Code

### Création d'une Définition d'Attribut

```csharp
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Geometry;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer une définition d'attribut
    AttributeDefinition attDef = new AttributeDefinition();
    attDef.SetDatabaseDefaults();
    attDef.Position = new Point3d(0, 0, 0);
    attDef.Tag = "NUMERO_PIECE";
    attDef.Prompt = "Entrer le numéro de pièce : ";
    attDef.TextString = "12345";
    attDef.Height = 2.5;
    attDef.Justify = AttachmentPoint.MiddleCenter;
    
    btr.AppendEntity(attDef);
    tr.AddNewlyCreatedDBObject(attDef, true);
    
    tr.Commit();
}
```

### Création d'un Bloc avec Attributs

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForWrite) as BlockTable;
    
    // Créer une nouvelle définition de bloc
    BlockTableRecord btr = new BlockTableRecord();
    btr.Name = "CARTOUCHE";
    btr.Origin = Point3d.Origin;
    
    ObjectId btrId = bt.Add(btr);
    tr.AddNewlyCreatedDBObject(btr, true);
    
    // Ajouter des définitions d'attributs
    AttributeDefinition attDef1 = new AttributeDefinition();
    attDef1.SetDatabaseDefaults();
    attDef1.Position = new Point3d(10, 40, 0);
    attDef1.Tag = "TITRE";
    attDef1.Prompt = "Entrer le titre du dessin : ";
    attDef1.TextString = "TITRE DU DESSIN";
    attDef1.Height = 5.0;
    
    btr.AppendEntity(attDef1);
    tr.AddNewlyCreatedDBObject(attDef1, true);
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Étiquettes Uniques**: Utiliser des étiquettes uniques et descriptives
2. **Invites Claires**: Fournir des invites utiles pour les utilisateurs
3. **Valeurs Par Défaut**: Définir des valeurs par défaut raisonnables
4. **Hauteur**: Utiliser des hauteurs de texte cohérentes
5. **Invisible vs Constant**: Utiliser invisible pour les données cachées, constant pour les valeurs fixes

## Classes Associées

- [AttributeReference](AttributeReference.md) - Instance d'attribut dans une référence de bloc
- [BlockReference](../Entities/Complex/BlockReference.md) - Insertion de bloc contenant des attributs
- [BlockTableRecord](../SymbolTables/BlockTable.md) - Définition de bloc contenant des définitions d'attributs

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
