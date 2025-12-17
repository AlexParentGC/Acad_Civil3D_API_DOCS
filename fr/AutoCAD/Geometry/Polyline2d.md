# Classe Polyline2d

## Vue d'Ensemble
La classe `Polyline2d` représente une entité spline linéaire par morceaux dans l'espace 2D (plan XY).

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Vertices` | `Point2dCollection` | Obtient les sommets |
| `NumVertices` | `int` | Obtient le nombre de sommets |
| `IsClosed` | `bool` | Vérifie si fermée |

## Exemples de Code

### Exemple 1: Créer une Polyligne 2D
```csharp
Point2dCollection vertices = new Point2dCollection();
vertices.Add(new Point2d(0, 0));
vertices.Add(new Point2d(10, 0));
vertices.Add(new Point2d(10, 10));
vertices.Add(new Point2d(0, 10));

Polyline2d polyline = new Polyline2d(vertices, true); // fermée

ed.WriteMessage($"\nPolyligne 2D : {polyline.NumVertices} sommets, fermée={polyline.IsClosed}");
```

## Classes Associées
- **Polyline3d** - Polyligne 3D
- **LineSegment2d** - Segments individuels

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
