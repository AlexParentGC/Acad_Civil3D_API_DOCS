# Classe Cylinder

## Vue d'Ensemble
La classe `Cylinder` représente une surface cylindrique circulaire droite bornée dans l'espace 3D. Les cylindres sont des primitives 3D courantes utilisées en conception mécanique et en modélisation.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Origin` | `Point3d` | Obtient le point de base du cylindre |
| `AxisOfSymmetry` | `Vector3d` | Obtient le vecteur de l'axe de rotation |
| `Radius` | `double` | Obtient le rayon de la base du cylindre |
| `Height` | `double` | Obtient la hauteur du cylindre |
| `ReferenceAxis` | `Vector3d` | Obtient le vecteur de référence sur le cercle de base |
| `IsOuterNormal` | `bool` | Indique si la normale de surface pointe vers l'extérieur |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la surface du cylindre |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à la surface |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur la surface |

## Exemples de Code

### Exemple 1: Créer des Cylindres
```csharp
// Cylindre avec base à l'origine, axe le long de Z, rayon 5, hauteur 10
Point3d baseCenter = Point3d.Origin;
Vector3d axis = Vector3d.ZAxis;
double radius = 5.0;
double height = 10.0;

Cylinder cylinder = new Cylinder(baseCenter, axis, radius, height);

ed.WriteMessage($"\nCylindre créé");
ed.WriteMessage($"\nBase : {cylinder.Origin}");
ed.WriteMessage($"\nAxe : {cylinder.AxisOfSymmetry}");
ed.WriteMessage($"\nRayon : {cylinder.Radius}");
ed.WriteMessage($"\nHauteur : {cylinder.Height}");
```

### Exemple 2: Calculs du Cylindre
```csharp
Cylinder cyl = new Cylinder(Point3d.Origin, Vector3d.ZAxis, 5.0, 10.0);

double lateralArea = 2 * Math.PI * cyl.Radius * cyl.Height;
double baseArea = Math.PI * Math.Pow(cyl.Radius, 2);
double totalSurfaceArea = lateralArea + 2 * baseArea;
double volume = baseArea * cyl.Height;

ed.WriteMessage($"\nAire de surface latérale : {lateralArea:F2}");
ed.WriteMessage($"\nAire de surface totale : {totalSurfaceArea:F2}");
ed.WriteMessage($"\nVolume : {volume:F2}");
```

### Exemple 3: Cylindre Horizontal
```csharp
// Cylindre le long de l'axe X
Point3d baseCenter = new Point3d(0, 5, 5);
Vector3d axis = Vector3d.XAxis; // Horizontal
double radius = 3.0;
double height = 15.0;

Cylinder horizontalCyl = new Cylinder(baseCenter, axis, radius, height);

Point3d topCenter = new Point3d(
    baseCenter.X + height * axis.X,
    baseCenter.Y + height * axis.Y,
    baseCenter.Z + height * axis.Z
);

ed.WriteMessage($"\nCylindre horizontal");
ed.WriteMessage($"\nCentre de base : {baseCenter}");
ed.WriteMessage($"\nCentre du sommet : {topCenter}");
```

## Bonnes Pratiques

1. **Axe** : Le vecteur d'axe doit être normalisé (vecteur unitaire)
2. **Hauteur** : Mesurée le long de l'axe depuis la base
3. **Orientation** : La base est au point d'origine, le sommet est origine + hauteur * axe
4. **Aire de Surface** : Latérale = 2πrh, Totale = 2πr(r + h)
5. **Volume** : V = πr²h

## Classes Associées
- **Sphere** - Surface sphérique
- **Cone** - Surface conique
- **Torus** - Surface toroïdale
- **Point3d** - Centre de base
- **Vector3d** - Direction de l'axe

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Cylinder](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Cylinder)
