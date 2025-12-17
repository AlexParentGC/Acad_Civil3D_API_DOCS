# Classe Torus

## Vue d'Ensemble
La classe `Torus` représente un segment toroïdal (forme de beignet) dans l'espace 3D. Les tores sont utilisés pour modéliser des tuyaux, des anneaux et des formes organiques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient le centre du tore |
| `AxisOfSymmetry` | `Vector3d` | Obtient l'axe de rotation |
| `MajorRadius` | `double` | Obtient la distance du centre à l'axe central du tube |
| `MinorRadius` | `double` | Obtient le rayon de la section transversale du tube |
| `ReferenceAxis` | `Vector3d` | Obtient l'axe de référence pour le tore |
| `IsApple` | `bool` | Vrai si rayon majeur < rayon mineur (forme de pomme) |
| `IsDoughnut` | `bool` | Vrai si rayon majeur > rayon mineur (forme de beignet) |
| `IsLemon` | `bool` | Vrai si rayon majeur = rayon mineur (forme de citron) |
| `IsDegenerate` | `bool` | Vrai si l'un des rayons <= 0 |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la surface du tore |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à la surface |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur la surface |
| `GetAnglesInU(...)` | `void` | Obtient les longitudes de départ/fin |
| `GetAnglesInV(...)` | `void` | Obtient les latitudes de départ/fin |

## Exemples de Code

### Exemple 1: Créer un Tore (Beignet)
```csharp
// Tore beignet standard
Point3d center = Point3d.Origin;
Vector3d axis = Vector3d.ZAxis;
double majorRadius = 10.0; // Distance du centre au centre du tube
double minorRadius = 3.0;  // Rayon du tube

Torus torus = new Torus(center, axis, majorRadius, minorRadius);

ed.WriteMessage($"\nTore créé");
ed.WriteMessage($"\nCentre : {torus.Center}");
ed.WriteMessage($"\nRayon majeur : {torus.MajorRadius}");
ed.WriteMessage($"\nRayon mineur : {torus.MinorRadius}");
ed.WriteMessage($"\nEst beignet : {torus.IsDoughnut}"); // true
ed.WriteMessage($"\nEst pomme : {torus.IsApple}"); // false
```

### Exemple 2: Types de Tores
```csharp
Point3d center = Point3d.Origin;
Vector3d axis = Vector3d.ZAxis;

// Beignet (majeur > mineur)
Torus doughnut = new Torus(center, axis, 10.0, 3.0);

// Pomme (majeur < mineur)
Torus apple = new Torus(center, axis, 3.0, 10.0);

// Citron (majeur = mineur)
Torus lemon = new Torus(center, axis, 5.0, 5.0);

ed.WriteMessage($"\nBeignet : IsDoughnut={doughnut.IsDoughnut}");
ed.WriteMessage($"\nPomme : IsApple={apple.IsApple}");
ed.WriteMessage($"\nCitron : IsLemon={lemon.IsLemon}");
```

### Exemple 3: Calculs du Tore
```csharp
Torus torus = new Torus(Point3d.Origin, Vector3d.ZAxis, 10.0, 3.0);

double R = torus.MajorRadius;
double r = torus.MinorRadius;

// Aire de surface = 4π²Rr
double surfaceArea = 4 * Math.PI * Math.PI * R * r;

// Volume = 2π²Rr²
double volume = 2 * Math.PI * Math.PI * R * r * r;

ed.WriteMessage($"\nAire de surface : {surfaceArea:F2}");
ed.WriteMessage($"\nVolume : {volume:F2}");
```

### Exemple 4: Tore Vertical
```csharp
// Tore avec axe le long de X (anneau vertical)
Point3d center = new Point3d(0, 5, 5);
Vector3d axis = Vector3d.XAxis;
double majorRadius = 8.0;
double minorRadius = 2.0;

Torus verticalTorus = new Torus(center, axis, majorRadius, minorRadius);

ed.WriteMessage($"\nTore vertical créé");
ed.WriteMessage($"\nAxe : {verticalTorus.AxisOfSymmetry}");
```

## Bonnes Pratiques

1. **Rayon Majeur** : Distance du centre du tore à l'axe central du tube
2. **Rayon Mineur** : Rayon du tube lui-même
3. **Beignet** : Le plus courant (majeur > mineur)
4. **Pomme** : Auto-intersectant (majeur < mineur)
5. **Citron** : Tore en fuseau (majeur = mineur)
6. **Aire de Surface** : 4π²Rr
7. **Volume** : 2π²Rr²

## Classes Associées
- **Sphere** - Surface sphérique
- **Cylinder** - Surface cylindrique
- **Cone** - Surface conique
- **Point3d** - Point central
- **Vector3d** - Direction de l'axe

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Torus](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Torus)
