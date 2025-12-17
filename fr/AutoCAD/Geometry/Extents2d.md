# Structure Extents2d

## Vue d'Ensemble
La structure `Extents2d` représente une boîte englobante 2D définie par des points minimum et maximum dans le plan XY.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `MinPoint` | `Point2d` | Obtient/définit le point de coin minimum |
| `MaxPoint` | `Point2d` | Obtient/définit le point de coin maximum |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `AddPoint(Point2d)` | `void` | Étend les limites pour inclure un point |
| `AddExtents(Extents2d)` | `void` | Étend les limites pour inclure d'autres limites |
| `ExpandBy(Vector2d)` | `void` | Étend les limites par un décalage vectoriel |
| `TransformBy(Matrix2d)` | `void` | Transforme les limites par une matrice |

## Exemples de Code

### Exemple 1: Créer des Limites 2D
```csharp
Point2d min = new Point2d(0, 0);
Point2d max = new Point2d(100, 50);
Extents2d extents = new Extents2d(min, max);

double width = max.X - min.X;
double height = max.Y - min.Y;

ed.WriteMessage($"\nLimites : {width} x {height}");
```

### Exemple 2: Ajouter des Points
```csharp
Extents2d extents = new Extents2d(new Point2d(0, 0), new Point2d(10, 10));

// Ajouter un point en dehors des limites actuelles
extents.AddPoint(new Point2d(15, 5));

ed.WriteMessage($"\nNouveau max : {extents.MaxPoint}"); // (15, 10)
```

## Classes Associées
- **Point2d** - Points de coin 2D
- **Vector2d** - Pour étendre les limites
- **Extents3d** - Boîte englobante 3D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
