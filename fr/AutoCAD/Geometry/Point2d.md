# Structure Point2d

## Vue d'Ensemble
La structure `Point2d` représente un point dans l'espace 2D avec des coordonnées X et Y. Elle est utilisée pour les opérations de géométrie planaire dans le plan XY.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `X` | `double` | Obtient la coordonnée X |
| `Y` | `double` | Obtient la coordonnée Y |
| `Origin` | `Point2d` (static) | Obtient le point d'origine (0, 0) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Add(Vector2d)` | `Point2d` | Ajoute un vecteur au point |
| `Subtract(Vector2d)` | `Point2d` | Soustrait un vecteur du point |
| `GetVectorTo(Point2d)` | `Vector2d` | Obtient le vecteur de ce point vers un autre |
| `DistanceTo(Point2d)` | `double` | Calcule la distance vers un autre point |
| `TransformBy(Matrix2d)` | `Point2d` | Transforme le point par une matrice |
| `RotateBy(double, Point2d)` | `Point2d` | Effectue une rotation du point autour d'un autre point |
| `Mirror(Line2d)` | `Point2d` | Effectue une symétrie du point à travers une ligne |
| `ToArray()` | `double[]` | Convertit en tableau [X, Y] |
| `IsEqualTo(Point2d)` | `bool` | Vérifie l'égalité |

## Exemples de Code

### Exemple 1: Créer des Points
```csharp
// Créer des points
Point2d origin = Point2d.Origin; // (0, 0)
Point2d pt1 = new Point2d(10, 20);
Point2d pt2 = new Point2d(5.5, 7.3);

// Depuis un tableau
double[] coords = { 15, 25 };
Point2d pt3 = new Point2d(coords);

ed.WriteMessage($"\nPoint 1 : ({pt1.X}, {pt1.Y})");
ed.WriteMessage($"\nPoint 2 : ({pt2.X}, {pt2.Y})");
```

### Exemple 2: Arithmétique de Points
```csharp
Point2d pt = new Point2d(10, 10);
Vector2d vec = new Vector2d(5, 3);

// Ajouter un vecteur
Point2d newPt = pt.Add(vec); // (15, 13)

// Soustraire un vecteur
Point2d movedPt = pt.Subtract(vec); // (5, 7)

// Obtenir le vecteur entre les points
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 10);
Vector2d direction = pt1.GetVectorTo(pt2);

ed.WriteMessage($"\nDirection : ({direction.X}, {direction.Y})");
```

### Exemple 3: Calculs de Distance
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 0);
Point2d pt3 = new Point2d(0, 10);

double dist12 = pt1.DistanceTo(pt2); // 10.0
double dist13 = pt1.DistanceTo(pt3); // 10.0
double dist23 = pt2.DistanceTo(pt3); // 14.14

ed.WriteMessage($"\nDistance pt1 à pt2 : {dist12:F2}");
ed.WriteMessage($"\nDistance pt2 à pt3 : {dist23:F2}");
```

### Exemple 4: Transformations
```csharp
Point2d pt = new Point2d(10, 0);

// Rotation de 90°
Point2d rotated = pt.RotateBy(Math.PI / 2, Point2d.Origin);

// Transformation par matrice
Matrix2d translation = Matrix2d.Displacement(new Vector2d(5, 10));
Point2d translated = pt.TransformBy(translation);

ed.WriteMessage($"\nRotation : ({rotated.X:F2}, {rotated.Y:F2})");
ed.WriteMessage($"\nTranslation : ({translated.X}, {translated.Y})");
```

### Exemple 5: Conversion vers 3D
```csharp
Point2d pt2d = new Point2d(10, 20);

// Convertir en 3D (Z = 0)
Point3d pt3d = new Point3d(pt2d.X, pt2d.Y, 0);

ed.WriteMessage($"\n2D : ({pt2d.X}, {pt2d.Y})");
ed.WriteMessage($"\n3D : ({pt3d.X}, {pt3d.Y}, {pt3d.Z})");

// Convertir de 3D vers 2D
Point3d pt3dInput = new Point3d(5, 10, 15);
Point2d pt2dResult = new Point2d(pt3dInput.X, pt3dInput.Y);

ed.WriteMessage($"\n3D vers 2D : ({pt2dResult.X}, {pt2dResult.Y})");
```

## Modèles Courants

### Calcul du Point Milieu
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(20, 10);

Point2d midpoint = new Point2d(
    (pt1.X + pt2.X) / 2,
    (pt1.Y + pt2.Y) / 2
);

// Alternative utilisant les vecteurs
Vector2d halfVec = pt1.GetVectorTo(pt2) * 0.5;
Point2d midpoint2 = pt1.Add(halfVec);
```

### Interpolation
```csharp
Point2d start = new Point2d(0, 0);
Point2d end = new Point2d(10, 10);

// Obtenir le point à 30% le long de la ligne
double t = 0.3;
Vector2d vec = start.GetVectorTo(end) * t;
Point2d interpolated = start.Add(vec);
```

## Bonnes Pratiques

1. **Utiliser pour 2D** : Préférer Point2d à Point3d pour les opérations planaires
2. **Origine** : Utiliser `Point2d.Origin` au lieu de `new Point2d(0, 0)`
3. **Immutabilité** : Point2d est une structure ; les opérations retournent de nouveaux points
4. **Tolérance** : Utiliser l'égalité basée sur la tolérance pour les comparaisons
5. **Conversion** : Conversion facile vers/depuis Point3d au besoin

## Classes Associées
- **Point3d** - Point 3D avec coordonnées X, Y, Z
- **Vector2d** - Direction et magnitude 2D
- **Matrix2d** - Matrice de transformation 2D
- **Line2d** - Ligne 2D utilisant deux points

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Point2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Point2d)
