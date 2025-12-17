# Classe NurbCurve2d

## Vue d'Ensemble
La classe `NurbCurve2d` représente une courbe B-spline rationnelle non uniforme (NURBS) dans l'espace 2D (plan XY). Comme son homologue 3D, elle fournit une représentation mathématique précise de courbes complexes en utilisant des points de contrôle, des nœuds et des poids.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ControlPoints` | `Point2dCollection` | Obtient les points de contrôle qui définissent la forme de la courbe |
| `Knots` | `KnotCollection` | Obtient le vecteur de nœuds |
| `Weights` | `DoubleCollection` | Obtient les poids pour les NURBS rationnelles |
| `Degree` | `int` | Obtient le degré polynomial de la courbe |
| `NumControlPoints` | `int` | Obtient le nombre de points de contrôle |
| `IsRational` | `bool` | Vérifie si la courbe a des poids |
| `IsPeriodic` | `bool` | Vérifie si la courbe est périodique (fermée) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `EvaluatePoint(double)` | `Point2d` | Évalue un point sur la courbe au paramètre |
| `GetClosestPointTo(Point2d)` | `Point2d` | Obtient le point le plus proche sur la courbe |
| `GetDistanceTo(Point2d)` | `double` | Obtient la distance d'un point à la courbe |
| `GetDerivativeAt(double)` | `Vector2d` | Obtient la tangente au paramètre |

## Exemples de Code

### Exemple 1: Créer une Courbe NURBS 2D
```csharp
Point2dCollection controlPoints = new Point2dCollection();
controlPoints.Add(new Point2d(0, 0));
controlPoints.Add(new Point2d(10, 5));
controlPoints.Add(new Point2d(20, 5));
controlPoints.Add(new Point2d(30, 0));

DoubleCollection knots = new DoubleCollection();
knots.Add(0); knots.Add(0); knots.Add(0); knots.Add(0);
knots.Add(1); knots.Add(1); knots.Add(1); knots.Add(1);

NurbCurve2d curve = new NurbCurve2d(3, knots, controlPoints, false);

ed.WriteMessage($"\nNURBS 2D créée avec {curve.NumControlPoints} points de contrôle");
```

### Exemple 2: Évaluer une NURBS 2D
```csharp
NurbCurve2d curve = /* courbe existante */;

for (int i = 0; i <= 10; i++)
{
    double param = i / 10.0;
    Point2d pt = curve.EvaluatePoint(param);
    Vector2d tangent = curve.GetDerivativeAt(param);
    
    ed.WriteMessage($"\nParam {param:F1} : ({pt.X:F2}, {pt.Y:F2}), Tangente : ({tangent.X:F2}, {tangent.Y:F2})");
}
```

## Bonnes Pratiques

1. **Opérations 2D** : Utiliser NurbCurve2d pour la géométrie planaire
2. **Vecteur de Nœuds** : Mêmes règles que 3D (longueur = numPointsDeContrôle + degré + 1)
3. **Performance** : Les NURBS 2D sont plus rapides que 3D pour le travail planaire

## Classes Associées
- **NurbCurve3d** - Courbe NURBS 3D
- **CubicSplineCurve2d** - Spline cubique 2D
- **Point2d** - Points de contrôle
- **Vector2d** - Vecteurs tangents

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe NurbCurve2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_NurbCurve2d)
