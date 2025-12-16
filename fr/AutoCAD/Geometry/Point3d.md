# Struct Point3d

## Vue d'Ensemble
La structure `Point3d` représente un point dans un espace 3D avec des coordonnées X, Y, et Z. C'est l'une des classes géométriques les plus fondamentales de l'API AutoCAD .NET, utilisée intensivement pour positionner des entités, définir des sommets et effectuer des calculs géométriques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `X` | `double` | Obtient la coordonnée X |
| `Y` | `double` | Obtient la coordonnée Y |
| `Z` | `double` | Obtient la coordonnée Z |
| `Origin` | `Point3d` (statique) | Obtient le point d'origine (0, 0, 0) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Add(Vector3d)` | `Point3d` | Ajoute un vecteur au point |
| `Subtract(Vector3d)` | `Point3d` | Soustrait un vecteur du point |
| `GetVectorTo(Point3d)` | `Vector3d` | Obtient le vecteur de ce point vers un autre |
| `DistanceTo(Point3d)` | `double` | Calcule la distance vers un autre point |
| `TransformBy(Matrix3d)` | `Point3d` | Transforme le point par une matrice |
| `RotateBy(double, Vector3d, Point3d)` | `Point3d` | Fait tourner le point autour d'un axe |
| `Mirror(Plane)` | `Point3d` | Miroite le point à travers un plan |
| `ToArray()` | `double[]` | Convertit en tableau [X, Y, Z] |

## Exemples de Code

### Exemple 1: Création de Points
```csharp
// Créer un point à l'origine
Point3d origin = Point3d.Origin; // (0, 0, 0)

// Créer un point avec des coordonnées
Point3d pt1 = new Point3d(10, 20, 0);
Point3d pt2 = new Point3d(5.5, 7.3, 12.8);

// Créer à partir d'un tableau
double[] coords = { 15, 25, 35 };
Point3d pt3 = new Point3d(coords);

ed.WriteMessage($"\nPoint 1 : ({pt1.X}, {pt1.Y}, {pt1.Z})");
ed.WriteMessage($"\nPoint 2 : ({pt2.X}, {pt2.Y}, {pt2.Z})");
ed.WriteMessage($"\nPoint 3 : ({pt3.X}, {pt3.Y}, {pt3.Z})");
```

### Exemple 2: Arithmétique de Point
```csharp
Point3d pt = new Point3d(10, 10, 0);
Vector3d vec = new Vector3d(5, 3, 0);

// Ajouter vecteur au point
Point3d newPt = pt.Add(vec); // (15, 13, 0)

// Soustraire vecteur du point
Point3d movedPt = pt.Subtract(vec); // (5, 7, 0)

// Obtenir vecteur entre deux points
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 10, 0);
Vector3d direction = pt1.GetVectorTo(pt2);

ed.WriteMessage($"\nOriginal : {pt}");
ed.WriteMessage($"\nAprès ajout : {newPt}");
ed.WriteMessage($"\nAprès soustraction : {movedPt}");
ed.WriteMessage($"\nVecteur direction : {direction}");
```

### Exemple 3: Calculs de Distance
```csharp
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 0, 0);
Point3d pt3 = new Point3d(0, 10, 0);
Point3d pt4 = new Point3d(10, 10, 10);

// Calculer les distances
double dist12 = pt1.DistanceTo(pt2); // 10.0
double dist13 = pt1.DistanceTo(pt3); // 10.0
double dist14 = pt1.DistanceTo(pt4); // 17.32 (distance 3D)

ed.WriteMessage($"\nDistance pt1 vers pt2 : {dist12:F2}");
ed.WriteMessage($"\nDistance pt1 vers pt3 : {dist13:F2}");
ed.WriteMessage($"\nDistance pt1 vers pt4 : {dist14:F2}");
```

### Exemple 4: Calcul de Point Médian
```csharp
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(20, 10, 5);

// Calculer le point médian
Point3d midpoint = new Point3d(
    (pt1.X + pt2.X) / 2,
    (pt1.Y + pt2.Y) / 2,
    (pt1.Z + pt2.Z) / 2
);

// Alternative utilisant vecteurs
Vector3d halfVector = pt1.GetVectorTo(pt2) * 0.5;
Point3d midpoint2 = pt1.Add(halfVector);

ed.WriteMessage($"\nPoint médian : ({midpoint.X}, {midpoint.Y}, {midpoint.Z})");
```

### Exemple 5: Transformation de Points
```csharp
Point3d pt = new Point3d(10, 0, 0);

// Créer matrice de translation (déplacer de 5 unités en X, 10 en Y)
Matrix3d translation = Matrix3d.Displacement(new Vector3d(5, 10, 0));
Point3d translatedPt = pt.TransformBy(translation);

// Créer matrice de rotation (tourner de 90° autour de l'axe Z)
Matrix3d rotation = Matrix3d.Rotation(Math.PI / 2, Vector3d.ZAxis, Point3d.Origin);
Point3d rotatedPt = pt.TransformBy(rotation);

// Créer matrice de mise à l'échelle (échelle par 2)
Matrix3d scaling = Matrix3d.Scaling(2.0, Point3d.Origin);
Point3d scaledPt = pt.TransformBy(scaling);

ed.WriteMessage($"\nOriginal : {pt}");
ed.WriteMessage($"\nTranslaté : {translatedPt}");
ed.WriteMessage($"\nTourné : {rotatedPt}");
ed.WriteMessage($"\nMis à l'échelle : {scaledPt}");
```

### Exemple 6: Rotation de Points
```csharp
Point3d pt = new Point3d(10, 0, 0);
Point3d center = Point3d.Origin;

// Tourner de 45° autour de l'axe Z
double angle = Math.PI / 4; // 45 degrés en radians
Point3d rotated = pt.RotateBy(angle, Vector3d.ZAxis, center);

ed.WriteMessage($"\nOriginal : ({pt.X:F2}, {pt.Y:F2}, {pt.Z:F2})");
ed.WriteMessage($"\nTourné 45° : ({rotated.X:F2}, {rotated.Y:F2}, {rotated.Z:F2})");

// Tourner plusieurs fois pour créer un motif circulaire
for (int i = 0; i < 8; i++)
{
    double ang = (i * 2 * Math.PI) / 8;
    Point3d circPt = pt.RotateBy(ang, Vector3d.ZAxis, center);
    ed.WriteMessage($"\nPoint {i} : ({circPt.X:F2}, {circPt.Y:F2})");
}
```

### Exemple 7: Création d'Entités avec des Points
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer ligne utilisant deux points
    Point3d startPt = new Point3d(0, 0, 0);
    Point3d endPt = new Point3d(10, 10, 0);
    
    Line line = new Line(startPt, endPt);
    line.SetDatabaseDefaults();
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    // Créer cercle utilisant point central
    Point3d center = new Point3d(5, 5, 0);
    Circle circle = new Circle(center, Vector3d.ZAxis, 3.0);
    circle.SetDatabaseDefaults();
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    // Créer arc utilisant centre, début, et fin
    Point3d arcCenter = new Point3d(0, 0, 0);
    double radius = 5.0;
    double startAngle = 0;
    double endAngle = Math.PI / 2;
    
    Arc arc = new Arc(arcCenter, Vector3d.ZAxis, radius, startAngle, endAngle);
    arc.SetDatabaseDefaults();
    btr.AppendEntity(arc);
    tr.AddNewlyCreatedDBObject(arc, true);
    
    tr.Commit();
}
```

### Exemple 8: Conversion Tableau de Points
```csharp
Point3d pt = new Point3d(10, 20, 30);

// Convertir en tableau
double[] coords = pt.ToArray();

ed.WriteMessage("\nCoordonnées du point :");
ed.WriteMessage($"\n  X : {coords[0]}");
ed.WriteMessage($"\n  Y : {coords[1]}");
ed.WriteMessage($"\n  Z : {coords[2]}");

// Créer point depuis tableau
Point3d newPt = new Point3d(coords);

ed.WriteMessage($"\nPoint recréé : ({newPt.X}, {newPt.Y}, {newPt.Z})");
```

### Exemple 9: Vérification d'Égalité de Points
```csharp
Point3d pt1 = new Point3d(10, 20, 0);
Point3d pt2 = new Point3d(10, 20, 0);
Point3d pt3 = new Point3d(10.0001, 20, 0);

// Égalité exacte
bool exact = pt1 == pt2; // vrai
bool notExact = pt1 == pt3; // faux

// Égalité basée sur tolérance
Tolerance tol = new Tolerance(0.001, 0.001);
bool withinTolerance = pt1.IsEqualTo(pt3, tol); // vrai si différence < 0.001

ed.WriteMessage($"\nÉgalité exacte (pt1 == pt2) : {exact}");
ed.WriteMessage($"\nÉgalité exacte (pt1 == pt3) : {notExact}");
ed.WriteMessage($"\nDans la tolérance : {withinTolerance}");
```

### Exemple 10: Trouver le Point le Plus Proche sur une Ligne
```csharp
// Définir une ligne
Point3d lineStart = new Point3d(0, 0, 0);
Point3d lineEnd = new Point3d(10, 0, 0);

// Point à projeter
Point3d testPoint = new Point3d(5, 5, 0);

// Calculer le point le plus proche sur la ligne
Vector3d lineVector = lineStart.GetVectorTo(lineEnd);
Vector3d pointVector = lineStart.GetVectorTo(testPoint);

double dotProduct = lineVector.DotProduct(pointVector);
double lineLength = lineVector.Length;
double t = dotProduct / (lineLength * lineLength);

// Restreindre t à [0, 1] pour rester sur le segment de ligne
t = Math.Max(0, Math.Min(1, t));

Point3d closestPoint = lineStart.Add(lineVector * t);

ed.WriteMessage($"\nPoint test : {testPoint}");
ed.WriteMessage($"\nPoint le plus proche sur ligne : {closestPoint}");
ed.WriteMessage($"\nDistance : {testPoint.DistanceTo(closestPoint):F2}");
```

## Modèles Communs

### Créer une Grille de Points
```csharp
List<Point3d> gridPoints = new List<Point3d>();

for (int x = 0; x < 10; x++)
{
    for (int y = 0; y < 10; y++)
    {
        gridPoints.Add(new Point3d(x * 10, y * 10, 0));
    }
}
```

### Interpoler Entre des Points
```csharp
Point3d start = new Point3d(0, 0, 0);
Point3d end = new Point3d(10, 10, 0);

// Obtenir 10 points le long de la ligne
for (int i = 0; i <= 10; i++)
{
    double t = i / 10.0;
    Vector3d vec = start.GetVectorTo(end) * t;
    Point3d interpolated = start.Add(vec);
}
```

## Point3d vs Point2d

| Fonctionnalité | Point3d | Point2d |
|----------------|---------|---------|
| Dimensions | X, Y, Z | X, Y |
| Namespace | Geometry | Geometry |
| Cas d'Usage | Coordonnées 3D | Coordonnées 2D |
| Conversion | Peut convertir en Point2d | Peut convertir en Point3d (Z=0) |

## Meilleures Pratiques

1. **Utiliser Origin** : Utilisez `Point3d.Origin` au lieu de `new Point3d(0, 0, 0)`
2. **Immutabilité** : Point3d est une structure ; les opérations retournent de nouveaux points
3. **Tolérance** : Utilisez l'égalité basée sur la tolérance pour les comparaisons à virgule flottante
4. **Vecteurs** : Utilisez `GetVectorTo()` pour les calculs de direction et de distance
5. **Transformations** : Utilisez `TransformBy()` pour les opérations géométriques complexes
6. **Opérations 2D** : Définissez Z=0 pour les opérations 2D dans le plan XY
7. **Performance** : Point3d est un type valeur (struct), passé par valeur
8. **Vérifications Null** : Point3d ne peut pas être null (c'est une structure)

## Classes Associées
- **Vector3d** - Représente direction et magnitude
- **Point2d** - Point 2D (X, Y seulement)
- **Matrix3d** - Matrice de transformation
- **Line3d** - Ligne 3D utilisant deux points
- **Plane** - Plan défini par point et normale
- **Extents3d** - Boîte englobante utilisant points min/max

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Struct Point3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Point3d)
- [Namespace Géométrie](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
