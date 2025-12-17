# Structure LineSegment2d

## Vue d'Ensemble
La structure `LineSegment2d` représente un segment de ligne borné dans l'espace 2D (plan XY), défini par des points de début et de fin.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `StartPoint` | `Point2d` | Obtient le point de début du segment |
| `EndPoint` | `Point2d` | Obtient le point de fin du segment |
| `MidPoint` | `Point2d` | Obtient le point milieu du segment |
| `Direction` | `Vector2d` | Obtient le vecteur de direction du début à la fin |
| `Length` | `double` | Obtient la longueur du segment |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `LineSegment2d(Point2d, Point2d)` | Crée un segment depuis les points de début et de fin |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Obtient le point le plus proche sur le segment vers un point donné |
| `DistanceTo(Point2d)` | `double` | Obtient la distance d'un point au segment |
| `IsOn(Point2d)` | `bool` | Vérifie si un point est sur le segment |
| `IsParallelTo(LineSegment2d)` | `bool` | Vérifie si parallèle à un autre segment |
| `IsPerpendicularTo(LineSegment2d)` | `bool` | Vérifie si perpendiculaire à un autre segment |

## Exemples de Code

### Exemple 1: Créer des Segments de Ligne 2D
```csharp
Point2d start = new Point2d(0, 0);
Point2d end = new Point2d(10, 10);
LineSegment2d segment = new LineSegment2d(start, end);

ed.WriteMessage($"\nLongueur : {segment.Length:F2}");
ed.WriteMessage($"\nPoint milieu : ({segment.MidPoint.X:F2}, {segment.MidPoint.Y:F2})");
```

### Exemple 2: Calcul de Distance
```csharp
LineSegment2d segment = new LineSegment2d(
    new Point2d(0, 0),
    new Point2d(10, 0)
);

Point2d testPoint = new Point2d(5, 5);
double distance = segment.DistanceTo(testPoint);

ed.WriteMessage($"\nDistance : {distance:F2}");
```

## Classes Associées
- **LineSegment3d** - Segment de ligne 3D
- **Line2d** - Ligne 2D non bornée
- **Point2d** - Points 2D
- **Vector2d** - Vecteur de direction 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
