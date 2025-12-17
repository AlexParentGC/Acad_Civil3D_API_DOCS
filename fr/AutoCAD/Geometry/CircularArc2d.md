# Structure CircularArc2d

## Vue d'Ensemble
La structure `CircularArc2d` représente un arc circulaire dans l'espace 2D (plan XY), défini par un centre, un rayon et des angles de début/fin.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point2d` | Obtient le point central de l'arc |
| `Radius` | `double` | Obtient le rayon de l'arc |
| `StartAngle` | `double` | Obtient l'angle de début (radians) |
| `EndAngle` | `double` | Obtient l'angle de fin (radians) |
| `StartPoint` | `Point2d` | Obtient le point de début de l'arc |
| `EndPoint` | `Point2d` | Obtient le point de fin de l'arc |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `CircularArc2d(Point2d, double)` | Crée un cercle complet |
| `CircularArc2d(Point2d, double, double, double)` | Crée un arc avec angles de début/fin |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Obtient le point le plus proche sur l'arc vers un point donné |
| `DistanceTo(Point2d)` | `double` | Obtient la distance d'un point à l'arc |
| `IsOn(Point2d)` | `bool` | Vérifie si un point est sur l'arc |

## Exemples de Code

### Exemple 1: Créer des Arcs 2D
```csharp
Point2d center = new Point2d(0, 0);
double radius = 10.0;
double startAngle = 0;
double endAngle = Math.PI / 2; // 90 degrés

CircularArc2d arc = new CircularArc2d(center, radius, startAngle, endAngle);

ed.WriteMessage($"\nRayon de l'arc : {arc.Radius}");
ed.WriteMessage($"\nPoint de début : ({arc.StartPoint.X:F2}, {arc.StartPoint.Y:F2})");
ed.WriteMessage($"\nPoint de fin : ({arc.EndPoint.X:F2}, {arc.EndPoint.Y:F2})");
```

## Classes Associées
- **CircularArc3d** - Arc circulaire 3D
- **Point2d** - Centre et points 2D
- **Arc** - Entité arc AutoCAD

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
