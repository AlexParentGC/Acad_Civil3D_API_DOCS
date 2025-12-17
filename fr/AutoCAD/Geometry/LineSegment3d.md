# Structure LineSegment3d

## Vue d'Ensemble
La structure `LineSegment3d` représente un segment de ligne borné dans l'espace 3D, défini par des points de début et de fin.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `StartPoint` | `Point3d` | Obtient le point de début du segment |
| `EndPoint` | `Point3d` | Obtient le point de fin du segment |
| `MidPoint` | `Point3d` | Obtient le point milieu du segment |
| `Direction` | `Vector3d` | Obtient le vecteur de direction du début à la fin |
| `Length` | `double` | Obtient la longueur du segment |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `LineSegment3d(Point3d, Point3d)` | Crée un segment depuis les points de début et de fin |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur le segment vers un point donné |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point au segment |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur le segment |
| `IsParallelTo(LineSegment3d)` | `bool` | Vérifie si parallèle à un autre segment |
| `IsPerpendicularTo(LineSegment3d)` | `bool` | Vérifie si perpendiculaire à un autre segment |

## Exemples de Code

### Exemple 1: Créer des Segments de Ligne
```csharp
Point3d start = new Point3d(0, 0, 0);
Point3d end = new Point3d(10, 10, 0);
LineSegment3d segment = new LineSegment3d(start, end);

ed.WriteMessage($"\nLongueur : {segment.Length:F2}");
ed.WriteMessage($"\nPoint milieu : {segment.MidPoint}");
ed.WriteMessage($"\nDirection : {segment.Direction}");
```

### Exemple 2: Distance Point-Segment
```csharp
LineSegment3d segment = new LineSegment3d(
    new Point3d(0, 0, 0),
    new Point3d(10, 0, 0)
);

Point3d testPoint = new Point3d(5, 5, 0);
double distance = segment.DistanceTo(testPoint);
Point3d closestPoint = segment.GetClosestPointTo(testPoint);

ed.WriteMessage($"\nDistance au segment : {distance:F2}");
ed.WriteMessage($"\nPoint le plus proche : {closestPoint}");
```

### Exemple 3: Vérifier si un Point est sur le Segment
```csharp
LineSegment3d segment = new LineSegment3d(
    new Point3d(0, 0, 0),
    new Point3d(10, 0, 0)
);

Point3d pt1 = new Point3d(5, 0, 0); // Sur le segment
Point3d pt2 = new Point3d(15, 0, 0); // Au-delà de la fin
Point3d pt3 = new Point3d(5, 5, 0); // Hors du segment

bool isOn1 = segment.IsOn(pt1); // true
bool isOn2 = segment.IsOn(pt2); // false
bool isOn3 = segment.IsOn(pt3); // false

ed.WriteMessage($"\nPoint 1 sur le segment : {isOn1}");
ed.WriteMessage($"\nPoint 2 sur le segment : {isOn2}");
ed.WriteMessage($"\nPoint 3 sur le segment : {isOn3}");
```

## Classes Associées
- **Line3d** - Ligne 3D non bornée
- **Point3d** - Points de début/fin
- **Vector3d** - Vecteur de direction
- **LineSegment2d** - Segment de ligne 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
