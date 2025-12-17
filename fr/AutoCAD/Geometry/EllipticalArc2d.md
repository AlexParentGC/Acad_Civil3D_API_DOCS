# Classe EllipticalArc2d

## Vue d'Ensemble
La classe `EllipticalArc2d` représente des ellipses complètes et des arcs elliptiques dans l'espace 2D (plan XY). Elle fournit les mêmes fonctionnalités que `EllipticalArc3d` mais optimisée pour la géométrie planaire.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point2d` | Obtient le point central |
| `MajorRadius` | `double` | Obtient la longueur du rayon majeur |
| `MinorRadius` | `double` | Obtient la longueur du rayon mineur |
| `StartAngle` | `double` | Obtient l'angle de départ (radians) |
| `EndAngle` | `double` | Obtient l'angle de fin (radians) |
| `StartPoint` | `Point2d` | Obtient le point de départ |
| `EndPoint` | `Point2d` | Obtient le point de fin |
| `IsCircular` | `bool` | Vérifie si l'ellipse est un cercle |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Obtient le point le plus proche sur l'ellipse |
| `DistanceTo(Point2d)` | `double` | Obtient la distance d'un point |
| `IsOn(Point2d)` | `bool` | Vérifie si un point est sur l'ellipse |

## Exemples de Code

### Exemple 1: Créer des Ellipses 2D
```csharp
Point2d center = new Point2d(0, 0);
double majorRadius = 10.0;
double minorRadius = 5.0;

// Ellipse complète
EllipticalArc2d ellipse = new EllipticalArc2d(center, majorRadius, minorRadius, 0, 2 * Math.PI);

ed.WriteMessage($"\nEllipse 2D : Majeur={ellipse.MajorRadius}, Mineur={ellipse.MinorRadius}");
ed.WriteMessage($"\nEst circulaire : {ellipse.IsCircular}");
```

### Exemple 2: Créer des Arcs Elliptiques
```csharp
Point2d center = new Point2d(10, 10);
double majorRadius = 8.0;
double minorRadius = 4.0;
double startAngle = 0;
double endAngle = Math.PI; // 180°

EllipticalArc2d arc = new EllipticalArc2d(center, majorRadius, minorRadius, startAngle, endAngle);

ed.WriteMessage($"\nArc de {startAngle * 180 / Math.PI}° à {endAngle * 180 / Math.PI}°");
ed.WriteMessage($"\nDépart : ({arc.StartPoint.X:F2}, {arc.StartPoint.Y:F2})");
ed.WriteMessage($"\nFin : ({arc.EndPoint.X:F2}, {arc.EndPoint.Y:F2})");
```

## Bonnes Pratiques

1. **Performance 2D** : Utiliser EllipticalArc2d pour le travail planaire (plus rapide que 3D)
2. **Ordre des Rayons** : Rayon majeur >= rayon mineur
3. **Angles** : En radians, pas en degrés

## Classes Associées
- **EllipticalArc3d** - Arc elliptique 3D
- **CircularArc2d** - Arc circulaire 2D
- **Point2d** - Points 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe EllipticalArc2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_EllipticalArc2d)
