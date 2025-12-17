# Structure CircularArc3d

## Vue d'Ensemble
La structure `CircularArc3d` représente un arc circulaire dans l'espace 3D, défini par un centre, une normale, un rayon et des angles de début/fin.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient le point central de l'arc |
| `Normal` | `Vector3d` | Obtient le vecteur normal perpendiculaire au plan de l'arc |
| `Radius` | `double` | Obtient le rayon de l'arc |
| `StartAngle` | `double` | Obtient l'angle de début (radians) |
| `EndAngle` | `double` | Obtient l'angle de fin (radians) |
| `StartPoint` | `Point3d` | Obtient le point de début de l'arc |
| `EndPoint` | `Point3d` | Obtient le point de fin de l'arc |
| `ReferenceVector` | `Vector3d` | Obtient le vecteur de référence pour la mesure d'angle |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `CircularArc3d(Point3d, Vector3d, double)` | Crée un cercle complet |
| `CircularArc3d(Point3d, Vector3d, Vector3d, double, double, double)` | Crée un arc avec tous les paramètres |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur l'arc vers un point donné |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à l'arc |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur l'arc |

## Exemples de Code

### Exemple 1: Créer des Arcs Circulaires
```csharp
Point3d center = new Point3d(0, 0, 0);
Vector3d normal = Vector3d.ZAxis;
double radius = 10.0;

// Cercle complet
CircularArc3d circle = new CircularArc3d(center, normal, radius);

ed.WriteMessage($"\nRayon du cercle : {circle.Radius}");
ed.WriteMessage($"\nCentre du cercle : {circle.Center}");
```

## Classes Associées
- **CircularArc2d** - Arc circulaire 2D
- **Point3d** - Centre et points sur l'arc
- **Vector3d** - Vecteur normal
- **Arc** - Entité arc AutoCAD

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
