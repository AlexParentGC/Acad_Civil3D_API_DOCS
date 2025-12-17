# Structure Line2d

## Vue d'Ensemble
La structure `Line2d` représente une ligne non bornée dans l'espace 2D (plan XY), définie par un point et un vecteur de direction.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Point` | `Point2d` | Obtient un point sur la ligne |
| `Direction` | `Vector2d` | Obtient le vecteur de direction de la ligne |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `Line2d(Point2d, Vector2d)` | Crée une ligne depuis un point et une direction |
| `Line2d(Point2d, Point2d)` | Crée une ligne passant par deux points |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Obtient le point le plus proche sur la ligne vers un point donné |
| `DistanceTo(Point2d)` | `double` | Obtient la distance d'un point à la ligne |
| `IsOn(Point2d)` | `bool` | Vérifie si un point est sur la ligne |
| `IsParallelTo(Line2d)` | `bool` | Vérifie si parallèle à une autre ligne |
| `IsPerpendicularTo(Line2d)` | `bool` | Vérifie si perpendiculaire à une autre ligne |
| `IntersectWith(Line2d)` | `Point2d` | Obtient le point d'intersection avec une autre ligne |

## Exemples de Code

### Exemple 1: Créer des Lignes 2D
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 10);
Line2d line = new Line2d(pt1, pt2);

ed.WriteMessage($"\nDirection de la ligne : {line.Direction}");
```

### Exemple 2: Distance Point-Ligne
```csharp
Line2d line = new Line2d(new Point2d(0, 0), new Point2d(10, 0));
Point2d testPoint = new Point2d(5, 5);

double distance = line.DistanceTo(testPoint);
Point2d closestPoint = line.GetClosestPointTo(testPoint);

ed.WriteMessage($"\nDistance : {distance:F2}");
ed.WriteMessage($"\nPoint le plus proche : ({closestPoint.X:F2}, {closestPoint.Y:F2})");
```

## Classes Associées
- **LineSegment2d** - Segment de ligne 2D borné
- **Point2d** - Points 2D
- **Vector2d** - Vecteur de direction 2D
- **Line3d** - Ligne 3D non bornée

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
