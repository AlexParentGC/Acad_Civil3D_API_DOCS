# Transparency Class

## Vue d'Ensemble
La classe `Transparency` représente la transparence d'une entité AutoCAD.

## Namespace
`Autodesk.AutoCAD.Colors`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Alpha` | `byte` | Valeur alpha (0-255, 0=transparent, 255=opaque) |
| `IsByLayer` | `bool` | Vérifie si la transparence est ByLayer |
| `IsByBlock` | `bool` | Vérifie si la transparence est ByBlock |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(objectId, OpenMode.ForWrite) as Entity;
    
    // Définir la transparence à 50% (128/255)
    ent.Transparency = new Transparency(128);
    
    tr.Commit();
}
```

## Classes Associées
- **Color** - Classe de couleur
- **Entity** - Entité avec transparence

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
