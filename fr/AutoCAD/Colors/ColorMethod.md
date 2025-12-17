# ColorMethod Enumeration

## Vue d'Ensemble
L'énumération `ColorMethod` définit comment une couleur est spécifiée pour une entité AutoCAD.

## Namespace
`Autodesk.AutoCAD.Colors`

## Valeurs

| Valeur | Description |
|--------|-------------|
| `ByLayer` | La couleur est héritée du calque |
| `ByBlock` | La couleur est héritée du bloc |
| `ByAci` | La couleur est définie par l'index ACI |
| `ByColor` | La couleur est définie par les valeurs RGB |
| `ByPen` | La couleur est définie par le numéro de plume |
| `Foreground` | Utilise la couleur de premier plan |
| `ByDgnLs` | Couleur par style de ligne DGN |
| `None` | Aucune méthode de couleur |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Colors;

Color color = Color.FromColorIndex(ColorMethod.ByLayer, 0);

if (color.ColorMethod == ColorMethod.ByLayer)
{
    ed.WriteMessage("\nLa couleur est ByLayer");
}
else if (color.ColorMethod == ColorMethod.ByAci)
{
    ed.WriteMessage($"\nCouleur ACI : {color.ColorIndex}");
}
else if (color.ColorMethod == ColorMethod.ByColor)
{
    ed.WriteMessage($"\nCouleur RGB : ({color.Red}, {color.Green}, {color.Blue})");
}
```

## Classes Associées
- **Color** - Classe de couleur
- **EntityColor** - Couleur d'entité

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
