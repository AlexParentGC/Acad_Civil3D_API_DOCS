# Classe EllipticalArc3d

## Vue d'Ensemble
La classe `EllipticalArc3d` représente des ellipses complètes et des arcs elliptiques dans l'espace 3D. Les ellipses sont des formes géométriques fondamentales en CAO, couramment utilisées pour les trous, les rainures et les courbes organiques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient le point central de l'ellipse |
| `MajorAxis` | `Vector3d` | Obtient le vecteur de l'axe majeur (vecteur unitaire) |
| `MinorAxis` | `Vector3d` | Obtient le vecteur de l'axe mineur (vecteur unitaire) |
| `MajorRadius` | `double` | Obtient la longueur du rayon majeur |
| `MinorRadius` | `double` | Obtient la longueur du rayon mineur |
| `Normal` | `Vector3d` | Obtient le vecteur normal au plan de l'ellipse |
| `StartAngle` | `double` | Obtient l'angle de départ (radians) |
| `EndAngle` | `double` | Obtient l'angle de fin (radians) |
| `StartPoint` | `Point3d` | Obtient le point de départ de l'arc |
| `EndPoint` | `Point3d` | Obtient le point de fin de l'arc |
| `IsCircular` | `bool` | Vérifie si l'ellipse est en fait un cercle |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur l'ellipse |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point à l'ellipse |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur l'ellipse |
| `GetPlane()` | `Plane` | Obtient le plan infini contenant l'ellipse |
| `SetAngles(double, double)` | `void` | Change les angles de départ et de fin |
| `SetAxes(Vector3d, Vector3d)` | `void` | Redéfinit les axes majeur et mineur |
| `IntersectWith(...)` | `Point3d[]` | Calcule les intersections |

## Exemples de Code

### Exemple 1: Créer des Ellipses
```csharp
// Ellipse complète
Point3d center = new Point3d(0, 0, 0);
Vector3d majorAxis = new Vector3d(10, 0, 0);
Vector3d minorAxis = new Vector3d(0, 5, 0);
Vector3d normal = Vector3d.ZAxis;

EllipticalArc3d fullEllipse = new EllipticalArc3d(center, normal, majorAxis, minorAxis, 0, 2 * Math.PI);

ed.WriteMessage($"\nEllipse complète créée");
ed.WriteMessage($"\nRayon majeur : {fullEllipse.MajorRadius}");
ed.WriteMessage($"\nRayon mineur : {fullEllipse.MinorRadius}");
ed.WriteMessage($"\nEst circulaire : {fullEllipse.IsCircular}");
```

### Exemple 2: Créer des Arcs Elliptiques
```csharp
Point3d center = new Point3d(10, 10, 0);
Vector3d majorAxis = new Vector3d(8, 0, 0);
Vector3d minorAxis = new Vector3d(0, 4, 0);
Vector3d normal = Vector3d.ZAxis;

// Arc de 0° à 180°
double startAngle = 0;
double endAngle = Math.PI;

EllipticalArc3d arc = new EllipticalArc3d(center, normal, majorAxis, minorAxis, startAngle, endAngle);

ed.WriteMessage($"\nArc elliptique créé");
ed.WriteMessage($"\nPoint de départ : {arc.StartPoint}");
ed.WriteMessage($"\nPoint de fin : {arc.EndPoint}");
ed.WriteMessage($"\nAngle de l'arc : {(endAngle - startAngle) * 180 / Math.PI}°");
```

### Exemple 3: Vérifier si l'Ellipse est un Cercle
```csharp
EllipticalArc3d ellipse1 = new EllipticalArc3d(
    Point3d.Origin,
    Vector3d.ZAxis,
    new Vector3d(5, 0, 0),
    new Vector3d(0, 5, 0),
    0, 2 * Math.PI
);

EllipticalArc3d ellipse2 = new EllipticalArc3d(
    Point3d.Origin,
    Vector3d.ZAxis,
    new Vector3d(10, 0, 0),
    new Vector3d(0, 5, 0),
    0, 2 * Math.PI
);

ed.WriteMessage($"\nEllipse 1 est circulaire : {ellipse1.IsCircular}"); // true (5 = 5)
ed.WriteMessage($"\nEllipse 2 est circulaire : {ellipse2.IsCircular}"); // false (10 ≠ 5)
```

### Exemple 4: Modifier les Angles de l'Ellipse
```csharp
EllipticalArc3d arc = /* arc elliptique existant */;

// Changer en arc d'un quart (0° à 90°)
arc.SetAngles(0, Math.PI / 2);

ed.WriteMessage($"\nNouvel angle de départ : {arc.StartAngle * 180 / Math.PI}°");
ed.WriteMessage($"\nNouvel angle de fin : {arc.EndAngle * 180 / Math.PI}°");
ed.WriteMessage($"\nNouveau point de départ : {arc.StartPoint}");
ed.WriteMessage($"\nNouveau point de fin : {arc.EndPoint}");
```

### Exemple 5: Trouver le Point le Plus Proche sur l'Ellipse
```csharp
EllipticalArc3d ellipse = new EllipticalArc3d(
    new Point3d(0, 0, 0),
    Vector3d.ZAxis,
    new Vector3d(10, 0, 0),
    new Vector3d(0, 5, 0),
    0, 2 * Math.PI
);

Point3d testPoint = new Point3d(15, 3, 0);
Point3d closestPoint = ellipse.GetClosestPointTo(testPoint);
double distance = ellipse.DistanceTo(testPoint);

ed.WriteMessage($"\nPoint de test : {testPoint}");
ed.WriteMessage($"\nPoint le plus proche sur l'ellipse : {closestPoint}");
ed.WriteMessage($"\nDistance : {distance:F4}");
```

### Exemple 6: Créer une Ellipse dans un Plan Différent
```csharp
// Ellipse dans le plan YZ (vertical)
Point3d center = new Point3d(0, 10, 5);
Vector3d normal = Vector3d.XAxis; // Normale pointe le long de X
Vector3d majorAxis = new Vector3d(0, 8, 0); // Le long de Y
Vector3d minorAxis = new Vector3d(0, 0, 4); // Le long de Z

EllipticalArc3d verticalEllipse = new EllipticalArc3d(
    center, normal, majorAxis, minorAxis, 0, 2 * Math.PI
);

Plane ellipsePlane = verticalEllipse.GetPlane();

ed.WriteMessage($"\nEllipse verticale créée");
ed.WriteMessage($"\nNormale du plan : {ellipsePlane.Normal}");
ed.WriteMessage($"\nPoint du plan : {ellipsePlane.PointOnPlane}");
```

## Modèles Courants

### Créer une Ellipse à partir des Rayons
```csharp
Point3d center = new Point3d(0, 0, 0);
double majorRadius = 10.0;
double minorRadius = 5.0;
Vector3d normal = Vector3d.ZAxis;

// Axe majeur le long de X, mineur le long de Y
Vector3d majorAxis = new Vector3d(majorRadius, 0, 0);
Vector3d minorAxis = new Vector3d(0, minorRadius, 0);

EllipticalArc3d ellipse = new EllipticalArc3d(
    center, normal, majorAxis, minorAxis, 0, 2 * Math.PI
);
```

### Convertir une Ellipse en Cercle
```csharp
EllipticalArc3d ellipse = /* ellipse existante */;

if (ellipse.IsCircular)
{
    double radius = ellipse.MajorRadius; // ou MinorRadius, ils sont égaux
    CircularArc3d circle = new CircularArc3d(
        ellipse.Center,
        ellipse.Normal,
        radius
    );
}
```

## Bonnes Pratiques

1. **Majeur vs Mineur** : Le rayon majeur doit être >= rayon mineur
2. **Vérification Circulaire** : Utiliser la propriété `IsCircular` au lieu de comparer les rayons
3. **Angles** : Les angles sont en radians, pas en degrés
4. **Vecteur Normal** : S'assurer que la normale est normalisée (vecteur unitaire)
5. **Ellipse Complète** : Utiliser angleDépart = 0, angleFin = 2π pour une ellipse complète
6. **Plan** : L'ellipse se trouve dans le plan défini par le centre et la normale

## Classes Associées
- **EllipticalArc2d** - Arc elliptique 2D
- **CircularArc3d** - Arc circulaire (cas spécial)
- **Point3d** - Centre et points sur l'ellipse
- **Vector3d** - Axes et normale
- **Plane** - Plan contenant l'ellipse

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe EllipticalArc3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_EllipticalArc3d)
