# EntityColor Class

## Vue d'Ensemble
La classe `EntityColor` représente la couleur d'une entité AutoCAD, incluant les informations de transparence.

## Namespace
`Autodesk.AutoCAD.Colors`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ColorMethod` | `ColorMethod` | Méthode de couleur |
| `ColorIndex` | `short` | Index de couleur ACI |
| `Red` | `byte` | Composant rouge |
| `Green` | `byte` | Composant vert |
| `Blue` | `byte` | Composant bleu |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(objectId, OpenMode.ForRead) as Entity;
    EntityColor entColor = ent.EntityColor;
    
    ed.WriteMessage($"\nMéthode de couleur : {entColor.ColorMethod}");
    ed.WriteMessage($"\nIndex de couleur : {entColor.ColorIndex}");
    
    tr.Commit();
}
```

## Classes Associées
- **Color** - Classe de couleur
- **ColorMethod** - Méthode de couleur
- **Entity** - Entité avec couleur

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
