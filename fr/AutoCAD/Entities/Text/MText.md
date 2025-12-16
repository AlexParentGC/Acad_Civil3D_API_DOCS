# Classe MText

## Vue d'Ensemble
La classe `MText` représente un texte multi-lignes dans AutoCAD avec des capacités de formatage riches incluant polices, couleurs, empilement, et plus.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ MText
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Contents` | `string` | Obtient/définit le contenu du texte (avec codes de formatage) |
| `Text` | `string` | Obtient le texte brut sans formatage |
| `Location` | `Point3d` | Obtient/définit le point d'insertion |
| `TextHeight` | `double` | Obtient/définit la hauteur du texte |
| `Width` | `double` | Obtient/définit la largeur de la limite de texte |
| `ActualWidth` | `double` | Obtient la largeur réelle du texte |
| `ActualHeight` | `double` | Obtient la hauteur réelle du texte |
| `Rotation` | `double` | Obtient/définit l'angle de rotation (radians) |
| `Attachment` | `AttachmentPoint` | Obtient/définit le point d'attachement |
| `FlowDirection` | `FlowDirection` | Obtient/définit la direction du flux de texte |
| `TextStyleId` | `ObjectId` | Obtient/définit le style de texte |
| `LineSpacingFactor` | `double` | Obtient/définit le facteur d'espacement de ligne |
| `LineSpacingStyle` | `LineSpacingStyle` | Obtient/définit le style d'espacement de ligne |
| `BackgroundFill` | `bool` | Obtient/définit si le remplissage d'arrière-plan est activé |

## Exemples de Code

### Exemple 1: Créer un MText Simple
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MText mtext = new MText();
    mtext.Location = new Point3d(100, 100, 0);
    mtext.TextHeight = 5.0;
    mtext.Width = 200.0; // Largeur de limite de texte
    mtext.Contents = "Ceci est un texte multi-lignes.\\PIl peut s'étendre sur plusieurs lignes.";
    
    btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    tr.Commit();
}
```

### Exemple 2: Créer un MText Formaté
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MText mtext = new MText();
    mtext.Location = new Point3d(100, 100, 0);
    mtext.TextHeight = 5.0;
    mtext.Width = 300.0;
    
    // Utiliser les codes de formatage :
    // \\P = nouveau paragraphe
    // \\C# = couleur (1=rouge, 2=jaune, etc.)
    // \\f = police
    // \\H = multiplicateur de hauteur
    mtext.Contents = "\\C1;Texte Rouge\\C;\\PTexte Normal\\P\\H2x;Grand Texte";
    
    btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    tr.Commit();
}
```

### Exemple 3: Définir le Point d'Attachement
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MText mtext = new MText();
    mtext.Location = new Point3d(200, 200, 0);
    mtext.TextHeight = 5.0;
    mtext.Width = 150.0;
    mtext.Contents = "Texte Milieu Centre";
    
    // Définir l'attachement au milieu centre
    mtext.Attachment = AttachmentPoint.MiddleCenter;
    
    btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    tr.Commit();
}
```

## Codes de Formatage Courants

| Code | Description |
|------|-------------|
| `\\P` | Nouveau paragraphe (saut de ligne) |
| `\\p` | Nouveau paragraphe sans saut de ligne |
| `\\C#;` | Couleur (1-255) |
| `\\C;` | Réinitialiser à la couleur par défaut |
| `\\f` | Changement de police |
| `\\H#x;` | Multiplicateur de hauteur |
| `\\W#;` | Facteur de largeur |
| `\\Q#;` | Angle oblique |
| `\\T#;` | Espacement (Tracking) |
| `\\L` | Soulignement activé |
| `\\l` | Soulignement désactivé |
| `\\O` | Surlignement activé |
| `\\o` | Surlignement désactivé |

## Objets Associés
- [DBText](DBText.md) - Texte simple ligne
- [TextStyleTable](../../SymbolTables/TextStyleTable.md) - Définitions de style de texte
- [Entity](../../BaseClasses/Entity.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_MText)
