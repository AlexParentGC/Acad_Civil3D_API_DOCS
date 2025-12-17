# Classe Ray2d

## Vue d'Ensemble
La classe `Ray2d` représente une ligne semi-bornée dans l'espace 2D (plan XY), définie par un point de base et un vecteur de direction.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `BasePoint` | `Point2d` | Obtient le point de départ du rayon |
| `Direction` | `Vector2d` | Obtient le vecteur de direction du rayon |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Obtient le point le plus proche sur le rayon |
| `DistanceTo(Point2d)` | `double` | Obtient la distance d'un point au rayon |
| `IsOn(Point2d)` | `bool` | Vérifie si un point est sur le rayon |

## Exemples de Code

### Exemple 1: Créer des Rayons 2D
```csharp
Point2d basePoint = new Point2d(0, 0);
Vector2d direction = new Vector2d(1, 1).GetNormal();

Ray2d ray = new Ray2d(basePoint, direction);

ed.WriteMessage($"\nRayon 2D depuis {ray.BasePoint} dans la direction {ray.Direction}");
```

### Exemple 2: Lancer de Rayon 2D
```csharp
Ray2d ray = new Ray2d(new Point2d(0, 0), new Vector2d(1, 0));
Point2d testPoint = new Point2d(10, 5);

Point2d closestPoint = ray.GetClosestPointTo(testPoint);
double distance = ray.DistanceTo(testPoint);

ed.WriteMessage($"\nPoint le plus proche : ({closestPoint.X:F2}, {closestPoint.Y:F2})");
ed.WriteMessage($"\nDistance : {distance:F2}");
```

## Bonnes Pratiques

1. **Performance 2D** : Utiliser Ray2d pour le lancer de rayon planaire (plus rapide que 3D)
2. **Direction** : Normaliser le vecteur de direction
3. **Semi-Borné** : S'étend depuis le point de base dans une seule direction

## Classes Associées
- **Ray3d** - Ligne semi-bornée 3D
- **Line2d** - Ligne 2D non bornée
- **LineSegment2d** - Segment de ligne 2D borné

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Ray2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Ray2d)
