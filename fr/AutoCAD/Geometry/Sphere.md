# Classe Sphere

## Vue d'Ensemble
La classe `Sphere` représente une surface sphérique dans l'espace 3D. Les sphères sont des primitives 3D fondamentales utilisées en modélisation, détection de collision et calculs géométriques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient le point central de la sphère |
| `Radius` | `double` | Obtient le rayon de la sphère |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la surface de la sphère |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à la surface de la sphère |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur la surface de la sphère |
| `IsInside(Point3d)` | `bool` | Vérifie si un point est à l'intérieur de la sphère |

## Exemples de Code

### Exemple 1: Créer des Sphères
```csharp
// Créer une sphère à l'origine avec un rayon de 10
Point3d center = Point3d.Origin;
double radius = 10.0;

Sphere sphere = new Sphere(center, radius);

ed.WriteMessage($"\nSphère créée");
ed.WriteMessage($"\nCentre : {sphere.Center}");
ed.WriteMessage($"\nRayon : {sphere.Radius}");
ed.WriteMessage($"\nAire de surface : {4 * Math.PI * radius * radius:F2}");
ed.WriteMessage($"\nVolume : {(4.0 / 3.0) * Math.PI * Math.Pow(radius, 3):F2}");
```

### Exemple 2: Relation Point-Sphère
```csharp
Sphere sphere = new Sphere(new Point3d(0, 0, 0), 10.0);

Point3d insidePoint = new Point3d(5, 0, 0);
Point3d onSurface = new Point3d(10, 0, 0);
Point3d outsidePoint = new Point3d(15, 0, 0);

ed.WriteMessage($"\nLe point intérieur est à l'intérieur : {sphere.IsInside(insidePoint)}");
ed.WriteMessage($"\nLe point de surface est sur la surface : {sphere.IsOn(onSurface)}");
ed.WriteMessage($"\nDistance du point extérieur : {sphere.DistanceTo(outsidePoint):F2}");
```

### Exemple 3: Trouver le Point le Plus Proche sur la Sphère
```csharp
Sphere sphere = new Sphere(Point3d.Origin, 10.0);
Point3d testPoint = new Point3d(20, 20, 20);

Point3d closestPoint = sphere.GetClosestPointTo(testPoint);
double distance = sphere.DistanceTo(testPoint);

ed.WriteMessage($"\nPoint de test : {testPoint}");
ed.WriteMessage($"\nPoint le plus proche sur la sphère : {closestPoint}");
ed.WriteMessage($"\nDistance à la surface : {distance:F4}");
```

## Bonnes Pratiques

1. **Rayon** : Doit être positif
2. **Surface vs Volume** : Utiliser `IsOn()` pour la surface, `IsInside()` pour le volume
3. **Distance** : La distance est vers la surface, pas le centre
4. **Calculs** : Aire de surface = 4πr², Volume = (4/3)πr³

## Classes Associées
- **Cylinder** - Surface cylindrique
- **Cone** - Surface conique
- **Torus** - Surface toroïdale
- **Point3d** - Centre et points sur la sphère

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Sphere](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Sphere)
