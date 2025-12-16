# Classe DBText

## Vue d'Ensemble
La classe `DBText` représente un texte sur une seule ligne dans AutoCAD. C'est une entité texte simple avec des options de formatage de base.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ DBText
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `TextString` | `string` | Obtient/définit le contenu du texte |
| `Position` | `Point3d` | Obtient/définit le point d'insertion |
| `AlignmentPoint` | `Point3d` | Obtient/définit le point d'alignement |
| `Height` | `double` | Obtient/définit la hauteur du texte |
| `Rotation` | `double` | Obtient/définit l'angle de rotation (radians) |
| `WidthFactor` | `double` | Obtient/définit le facteur de largeur |
| `Oblique` | `double` | Obtient/définit l'angle oblique |
| `TextStyleId` | `ObjectId` | Obtient/définit le style de texte |
| `HorizontalMode` | `TextHorizontalMode` | Obtient/définit la justification horizontale |
| `VerticalMode` | `TextVerticalMode` | Obtient/définit la justification verticale |
| `IsMirroredInX` | `bool` | Obtient/définit la symétrie axe X |
| `IsMirroredInY` | `bool` | Obtient/définit la symétrie axe Y |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |
| `Thickness` | `double` | Obtient/définit l'épaisseur |

## Exemples de Code

### Exemple 1: Créer un Texte Simple
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    DBText text = new DBText();
    text.Position = new Point3d(100, 100, 0);
    text.Height = 5.0;
    text.TextString = "Bonjour AutoCAD !";
    
    btr.AppendEntity(text);
    tr.AddNewlyCreatedDBObject(text, true);
    
    tr.Commit();
}
```

### Exemple 2: Créer un Texte Justifié
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    DBText text = new DBText();
    text.Height = 5.0;
    text.TextString = "Texte Centré";
    
    // Définir la justification
    text.HorizontalMode = TextHorizontalMode.TextCenter;
    text.VerticalMode = TextVerticalMode.TextVerticalMid;
    
    // Le point d'alignement est utilisé quand la justification n'est pas gauche-ligne de base
    text.AlignmentPoint = new Point3d(200, 100, 0);
    
    btr.AppendEntity(text);
    tr.AddNewlyCreatedDBObject(text, true);
    
    tr.Commit();
}
```

### Exemple 3: Modifier les Propriétés du Texte
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBText text = tr.GetObject(textId, OpenMode.ForWrite) as DBText;
    
    // Changer le contenu du texte
    text.TextString = "Texte Modifié";
    
    // Changer la hauteur
    text.Height = 10.0;
    
    // Rotation de 45 degrés
    text.Rotation = Math.PI / 4;
    
    // Rendre le texte plus large
    text.WidthFactor = 1.5;
    
    // Appliquer un angle oblique (effet italique)
    text.Oblique = 15 * (Math.PI / 180);
    
    tr.Commit();
}
```

## Objets Associés
- [MText](MText.md) - Texte multi-lignes avec formatage riche
- [TextStyleTable](../../SymbolTables/TextStyleTable.md) - Définitions de style de texte
- [Entity](../../BaseClasses/Entity.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBText)
