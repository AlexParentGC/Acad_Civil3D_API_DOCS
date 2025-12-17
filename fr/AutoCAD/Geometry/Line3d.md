# Structure Line3d

## Vue d'Ensemble
La structure `Line3d` représente une ligne non bornée dans l'espace 3D, définie par un point et un vecteur de direction. Contrairement à la classe d'entité `Line`, `Line3d` s'étend à l'infini dans les deux directions.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Point` | `Point3d` | Obtient un point sur la ligne |
| `Direction` | `Vector3d` | Obtient le vecteur de direction de la ligne |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `Line3d(Point3d, Vector3d)` | Crée une ligne depuis un point et une direction |
| `Line3d(Point3d, Point3d)` | Crée une ligne passant par deux points |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la ligne vers un point donné |
| `GetClosestPointTo(Line3d)` | `Point3d` | Obtient le point le plus proche vers une autre ligne |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à la ligne |
| `DistanceTo(Line3d)` | `double` | Obtient la distance entre deux lignes |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur la ligne |
| `IsParallelTo(Line3d)` | `bool` | Vérifie si parallèle à une autre ligne |
| `IsPerpendicularTo(Line3d)` | `bool` | Vérifie si perpendiculaire à une autre ligne |
| `IsColinearTo(Line3d)` | `bool` | Vérifie si colinéaire avec une autre ligne |
| `IntersectWith(Line3d)` | `Point3d` | Obtient le point d'intersection avec une autre ligne |
| `IntersectWith(Plane)` | `Point3d` | Obtient le point d'intersection avec un plan |

## Exemples de Code

### Exemple 1: Créer des Lignes
```csharp
// Depuis un point et une direction
Point3d pt = new Point3d(0, 0, 0);
Vector3d dir = new Vector3d(1, 1, 0).GetNormal();
Line3d line1 = new Line3d(pt, dir);

// Depuis deux points
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 10, 0);
Line3d line2 = new Line3d(pt1, pt2);

ed.WriteMessage($"\nDirection de la ligne : {line2.Direction}");
```

### Exemple 2: Distance Point-Ligne
```csharp
Line3d line = new Line3d(new Point3d(0, 0, 0), new Point3d(10, 0, 0));
Point3d testPoint = new Point3d(5, 5, 0);

double distance = line.DistanceTo(testPoint);
Point3d closestPoint = line.GetClosestPointTo(testPoint);

ed.WriteMessage($"\nDistance à la ligne : {distance:F2}");
ed.WriteMessage($"\nPoint le plus proche : {closestPoint}");
```

### Exemple 3: Intersection de Lignes
```csharp
// Deux lignes qui se croisent
Line3d line1 = new Line3d(new Point3d(0, 0, 0), new Point3d(10, 0, 0));
Line3d line2 = new Line3d(new Point3d(5, -5, 0), new Point3d(5, 5, 0));

try
{
    Point3d intersection = line1.IntersectWith(line2);
    ed.WriteMessage($"\nIntersection à : {intersection}");
}
catch
{
    ed.WriteMessage("\nLes lignes ne se croisent pas");
}
```

### Exemple 4: Intersection Ligne-Plan
```csharp
Line3d line = new Line3d(new Point3d(0, 0, -10), new Point3d(0, 0, 10));
Plane plane = new Plane(Point3d.Origin, Vector3d.ZAxis);

try
{
    Point3d intersection = line.IntersectWith(plane);
    ed.WriteMessage($"\nLa ligne croise le plan à : {intersection}");
}
catch
{
    ed.WriteMessage("\nLa ligne ne croise pas le plan");
}
```

## Classes Associées
- **LineSegment3d** - Segment de ligne borné
- **Point3d** - Points sur la ligne
- **Vector3d** - Vecteur de direction
- **Plane** - Pour les intersections ligne-plan

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
