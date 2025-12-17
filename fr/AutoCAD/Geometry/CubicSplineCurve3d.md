# Classe CubicSplineCurve3d

## Vue d'Ensemble
La classe `CubicSplineCurve3d` représente une spline cubique d'interpolation dans l'espace 3D. Les splines cubiques créent des courbes lisses qui passent par des points d'ajustement spécifiés, couramment utilisées pour une interpolation lisse.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `FitPoints` | `Point3dCollection` | Obtient les points par lesquels la courbe passe |
| `StartTangent` | `Vector3d` | Obtient le vecteur tangent au départ |
| `EndTangent` | `Vector3d` | Obtient le vecteur tangent à la fin |
| `NumFitPoints` | `int` | Obtient le nombre de points d'ajustement |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `EvaluatePoint(double)` | `Point3d` | Évalue un point sur la courbe au paramètre |
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la courbe |
| `GetDerivativeAt(double)` | `Vector3d` | Obtient la tangente au paramètre |

## Exemples de Code

### Exemple 1: Créer une Spline Cubique
```csharp
// Créer une spline passant par des points d'ajustement
Point3dCollection fitPoints = new Point3dCollection();
fitPoints.Add(new Point3d(0, 0, 0));
fitPoints.Add(new Point3d(5, 5, 0));
fitPoints.Add(new Point3d(10, 3, 0));
fitPoints.Add(new Point3d(15, 7, 0));
fitPoints.Add(new Point3d(20, 5, 0));

CubicSplineCurve3d spline = new CubicSplineCurve3d(fitPoints);

ed.WriteMessage($"\nSpline cubique créée passant par {spline.NumFitPoints} points");
```

### Exemple 2: Spline avec Tangentes
```csharp
Point3dCollection fitPoints = new Point3dCollection();
fitPoints.Add(new Point3d(0, 0, 0));
fitPoints.Add(new Point3d(10, 10, 0));
fitPoints.Add(new Point3d(20, 0, 0));

// Spécifier les tangentes de départ et de fin
Vector3d startTangent = Vector3d.XAxis;
Vector3d endTangent = Vector3d.XAxis;

CubicSplineCurve3d spline = new CubicSplineCurve3d(fitPoints, startTangent, endTangent);

ed.WriteMessage($"\nSpline avec tangentes spécifiées");
ed.WriteMessage($"\nTangente de départ : {spline.StartTangent}");
ed.WriteMessage($"\nTangente de fin : {spline.EndTangent}");
```

## Bonnes Pratiques

1. **Points d'Ajustement** : La courbe passe par tous les points d'ajustement
2. **Lissage** : Les splines cubiques fournissent une continuité C² (lisse)
3. **Tangentes** : Spécifier les tangentes pour un meilleur contrôle aux extrémités
4. **Interpolation** : Utiliser pour des courbes lisses passant par des points connus

## Classes Associées
- **CubicSplineCurve2d** - Spline cubique 2D
- **NurbCurve3d** - Courbe NURBS (plus de contrôle)
- **Polyline3d** - Linéaire par morceaux (non lisse)

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe CubicSplineCurve3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_CubicSplineCurve3d)
