# Classe CubicSplineCurve2d

## Vue d'Ensemble
La classe `CubicSplineCurve2d` représente une spline cubique d'interpolation dans l'espace 2D (plan XY).

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `FitPoints` | `Point2dCollection` | Obtient les points d'ajustement |
| `StartTangent` | `Vector2d` | Obtient la tangente de départ |
| `EndTangent` | `Vector2d` | Obtient la tangente de fin |

## Exemples de Code

### Exemple 1: Créer une Spline Cubique 2D
```csharp
Point2dCollection fitPoints = new Point2dCollection();
fitPoints.Add(new Point2d(0, 0));
fitPoints.Add(new Point2d(5, 5));
fitPoints.Add(new Point2d(10, 3));
fitPoints.Add(new Point2d(15, 7));

CubicSplineCurve2d spline = new CubicSplineCurve2d(fitPoints);

ed.WriteMessage($"\nSpline cubique 2D créée");
```

## Classes Associées
- **CubicSplineCurve3d** - Spline cubique 3D
- **NurbCurve2d** - Courbe NURBS 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
