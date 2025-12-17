# Classe Cone

## Vue d'Ensemble
La classe `Cone` représente un cône circulaire droit borné dans l'espace 3D. Les cônes sont des primitives 3D fondamentales utilisées pour modéliser des formes coniques et des calculs géométriques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `BaseCenter` | `Point3d` | Obtient le centre de la base du cône |
| `Apex` | `Point3d` | Obtient l'apex (sommet) du cône |
| `AxisOfSymmetry` | `Vector3d` | Obtient l'axe de la base à l'apex |
| `BaseRadius` | `double` | Obtient le rayon de la base du cône |
| `Height` | `double` | Obtient la hauteur du cône |
| `HalfAngle` | `double` | Obtient le demi-angle du cône (radians) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la surface du cône |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à la surface |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur la surface |

## Exemples de Code

### Exemple 1: Créer des Cônes
```csharp
// Cône avec base à l'origine, apex à (0, 0, 10), rayon de base 5
Point3d baseCenter = Point3d.Origin;
Point3d apex = new Point3d(0, 0, 10);
double baseRadius = 5.0;

Cone cone = new Cone(baseCenter, apex, baseRadius);

double height = baseCenter.DistanceTo(apex);
double halfAngle = Math.Atan(baseRadius / height);

ed.WriteMessage($"\nCône créé");
ed.WriteMessage($"\nCentre de base : {cone.BaseCenter}");
ed.WriteMessage($"\nApex : {cone.Apex}");
ed.WriteMessage($"\nRayon de base : {cone.BaseRadius}");
ed.WriteMessage($"\nHauteur : {height:F2}");
ed.WriteMessage($"\nDemi-angle : {halfAngle * 180 / Math.PI:F2}°");
```

### Exemple 2: Calculs du Cône
```csharp
Cone cone = new Cone(Point3d.Origin, new Point3d(0, 0, 10), 5.0);

double height = 10.0;
double radius = 5.0;
double slantHeight = Math.Sqrt(height * height + radius * radius);

double baseArea = Math.PI * radius * radius;
double lateralArea = Math.PI * radius * slantHeight;
double totalSurfaceArea = baseArea + lateralArea;
double volume = (1.0 / 3.0) * baseArea * height;

ed.WriteMessage($"\nHauteur oblique : {slantHeight:F2}");
ed.WriteMessage($"\nAire de base : {baseArea:F2}");
ed.WriteMessage($"\nAire de surface latérale : {lateralArea:F2}");
ed.WriteMessage($"\nAire de surface totale : {totalSurfaceArea:F2}");
ed.WriteMessage($"\nVolume : {volume:F2}");
```

### Exemple 3: Tronc de Cône
```csharp
// Créer un cône puis calculer le tronc (cône tronqué)
Point3d baseCenter = Point3d.Origin;
Point3d apex = new Point3d(0, 0, 15);
double baseRadius = 6.0;

Cone fullCone = new Cone(baseCenter, apex, baseRadius);

// Tronc : couper à hauteur 10 (rayon supérieur sera 2)
double cutHeight = 10.0;
double fullHeight = 15.0;
double topRadius = baseRadius * (fullHeight - cutHeight) / fullHeight;

ed.WriteMessage($"\nParamètres du tronc :");
ed.WriteMessage($"\nRayon de base : {baseRadius}");
ed.WriteMessage($"\nRayon supérieur : {topRadius:F2}");
ed.WriteMessage($"\nHauteur : {cutHeight}");

// Volume du tronc = (1/3)πh(r1² + r1*r2 + r2²)
double frustumVolume = (1.0 / 3.0) * Math.PI * cutHeight * 
    (baseRadius * baseRadius + baseRadius * topRadius + topRadius * topRadius);

ed.WriteMessage($"\nVolume du tronc : {frustumVolume:F2}");
```

## Bonnes Pratiques

1. **Position de l'Apex** : L'apex ne doit pas être au centre de la base
2. **Rayon de Base** : Doit être positif
3. **Demi-Angle** : tan(demi-angle) = rayonBase / hauteur
4. **Aire de Surface** : Latérale = πrs (s = hauteur oblique), Totale = πr(r + s)
5. **Volume** : V = (1/3)πr²h
6. **Tronc** : Pour un cône tronqué, utiliser des calculs proportionnels

## Classes Associées
- **Cylinder** - Surface cylindrique (cône avec angle de 0°)
- **Sphere** - Surface sphérique
- **Point3d** - Centre de base et apex
- **Vector3d** - Direction de l'axe

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Cone](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Cone)
