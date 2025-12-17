# Color Class

## Vue d'Ensemble
La classe `Color` représente une couleur dans AutoCAD. Elle fournit des méthodes pour créer et manipuler des couleurs en utilisant divers modèles de couleur incluant ACI (AutoCAD Color Index), RGB et les nuanciers de couleurs.

## Namespace
`Autodesk.AutoCAD.Colors`

## Méthodes de Fabrique Statiques

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `FromColorIndex(ColorMethod, short)` | `Color` | Crée une couleur à partir de l'index ACI |
| `FromRgb(byte, byte, byte)` | `Color` | Crée une couleur à partir des valeurs RGB |
| `FromNames(string, string)` | `Color` | Crée une couleur à partir d'un nuancier |
| `FromEntityColor(EntityColor)` | `Color` | Crée une couleur à partir d'EntityColor |

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ColorMethod` | `ColorMethod` | Obtient la méthode de couleur (ByLayer, ByBlock, ByACI, ByColor) |
| `ColorIndex` | `short` | Obtient ou définit l'index de couleur ACI (1-255) |
| `Red` | `byte` | Obtient le composant rouge (0-255) |
| `Green` | `byte` | Obtient le composant vert (0-255) |
| `Blue` | `byte` | Obtient le composant bleu (0-255) |
| `IsByLayer` | `bool` | Vérifie si la couleur est ByLayer |
| `IsByBlock` | `bool` | Vérifie si la couleur est ByBlock |
| `IsByAci` | `bool` | Vérifie si la couleur est par index ACI |
| `IsByColor` | `bool` | Vérifie si la couleur est vraie couleur (RGB) |

## Exemples de Code

### Création de Couleurs par Index ACI
```csharp
using Autodesk.AutoCAD.Colors;

Color red = Color.FromColorIndex(ColorMethod.ByAci, 1);    // Rouge
Color yellow = Color.FromColorIndex(ColorMethod.ByAci, 2); // Jaune
Color green = Color.FromColorIndex(ColorMethod.ByAci, 3);  // Vert
Color cyan = Color.FromColorIndex(ColorMethod.ByAci, 4);   // Cyan
Color blue = Color.FromColorIndex(ColorMethod.ByAci, 5);   // Bleu
```

### Création de Couleurs RGB
```csharp
Color orange = Color.FromRgb(255, 165, 0);
Color purple = Color.FromRgb(128, 0, 128);
Color pink = Color.FromRgb(255, 192, 203);
```

### Couleurs ByLayer et ByBlock
```csharp
// Couleur ByLayer (hérite du calque)
Color byLayer = Color.FromColorIndex(ColorMethod.ByLayer, 0);

// Couleur ByBlock (hérite du bloc)
Color byBlock = Color.FromColorIndex(ColorMethod.ByBlock, 0);
```

## Index de Couleurs ACI Courants

| Index | Nom de Couleur |
|-------|----------------|
| 0 | ByBlock |
| 1 | Rouge |
| 2 | Jaune |
| 3 | Vert |
| 4 | Cyan |
| 5 | Bleu |
| 6 | Magenta |
| 7 | Blanc/Noir (dépend de l'arrière-plan) |
| 256 | ByLayer |

## Meilleures Pratiques

1. **ByLayer**: Utiliser ByLayer pour la plupart des entités pour maintenir l'organisation par calques
2. **Vraie Couleur**: Utiliser RGB pour une correspondance précise des couleurs
3. **Couleurs ACI**: Utiliser ACI pour la compatibilité avec les anciennes versions d'AutoCAD
4. **Nuanciers**: Utiliser pour les couleurs standard de l'industrie (Pantone, RAL, etc.)

## Classes Associées
- **EntityColor** - Propriétés de couleur d'entité
- **ColorMethod** - Énumération de méthode de couleur
- **Transparency** - Paramètres de transparence
- **Entity** - Classe de base avec propriété Color

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
