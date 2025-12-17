# Classe NurbCurve3d

## Vue d'Ensemble
La classe `NurbCurve3d` représente une courbe B-spline rationnelle non uniforme (NURBS) dans l'espace 3D. Les courbes NURBS sont fondamentales pour les systèmes CAO, fournissant des représentations mathématiques précises de courbes complexes à forme libre en utilisant des points de contrôle, des nœuds et des poids.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ControlPoints` | `Point3dCollection` | Obtient les points de contrôle qui définissent la forme de la courbe |
| `Knots` | `KnotCollection` | Obtient le vecteur de nœuds qui détermine les valeurs de paramètres |
| `Weights` | `DoubleCollection` | Obtient les poids pour les NURBS rationnelles (influence des points de contrôle) |
| `Degree` | `int` | Obtient le degré polynomial de la courbe |
| `NumControlPoints` | `int` | Obtient le nombre de points de contrôle |
| `NumKnots` | `int` | Obtient le nombre de nœuds dans le vecteur de nœuds |
| `NumWeights` | `int` | Obtient le nombre de poids (0 pour non rationnelle) |
| `IsPeriodic` | `bool` | Vérifie si la courbe est périodique (fermée) |
| `IsRational` | `bool` | Vérifie si la courbe est rationnelle (a des poids) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `EvaluatePoint(double)` | `Point3d` | Évalue un point sur la courbe à une valeur de paramètre |
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la courbe vers un point donné |
| `GetDistanceTo(Point3d)` | `double` | Obtient la distance d'un point à la courbe |
| `AddControlPointAt(int, Point3d)` | `void` | Ajoute un point de contrôle à l'index spécifié |
| `DeleteControlPointAt(int)` | `void` | Supprime le point de contrôle à l'index |
| `GetWeightAt(int)` | `double` | Obtient le poids à l'index spécifié |
| `SetWeightAt(int, double)` | `void` | Définit le poids à l'index spécifié |
| `InsertKnot(double)` | `void` | Insère un nœud à la valeur de paramètre |
| `AddKnot(double)` | `void` | Ajoute un nœud au vecteur de nœuds |
| `ElevateDegree(int)` | `void` | Augmente le degré de la courbe sans changer sa forme |
| `GetDerivativeAt(double)` | `Vector3d` | Obtient la dérivée (tangente) au paramètre |

## Exemples de Code

### Exemple 1: Créer une Courbe NURBS
```csharp
// Définir les points de contrôle
Point3dCollection controlPoints = new Point3dCollection();
controlPoints.Add(new Point3d(0, 0, 0));
controlPoints.Add(new Point3d(10, 5, 0));
controlPoints.Add(new Point3d(20, 5, 0));
controlPoints.Add(new Point3d(30, 0, 0));

// Définir le vecteur de nœuds (degré 3, 4 points de contrôle = 8 nœuds)
DoubleCollection knots = new DoubleCollection();
knots.Add(0); knots.Add(0); knots.Add(0); knots.Add(0);
knots.Add(1); knots.Add(1); knots.Add(1); knots.Add(1);

// Créer la courbe NURBS (degré 3)
int degree = 3;
NurbCurve3d nurbsCurve = new NurbCurve3d(degree, knots, controlPoints, false);

ed.WriteMessage($"\nCourbe NURBS créée");
ed.WriteMessage($"\nDegré : {nurbsCurve.Degree}");
ed.WriteMessage($"\nPoints de contrôle : {nurbsCurve.NumControlPoints}");
ed.WriteMessage($"\nNœuds : {nurbsCurve.NumKnots}");
```

### Exemple 2: Évaluer des Points sur la Courbe NURBS
```csharp
NurbCurve3d curve = /* courbe NURBS existante */;

// Évaluer des points le long de la courbe
int numSamples = 10;
for (int i = 0; i <= numSamples; i++)
{
    double param = i / (double)numSamples;
    Point3d pt = curve.EvaluatePoint(param);
    
    ed.WriteMessage($"\nParamètre {param:F2} : ({pt.X:F2}, {pt.Y:F2}, {pt.Z:F2})");
}

// Obtenir la tangente au point médian
double midParam = 0.5;
Vector3d tangent = curve.GetDerivativeAt(midParam);
ed.WriteMessage($"\nTangente à 0.5 : {tangent}");
```

### Exemple 3: Créer une NURBS Pondérée (Rationnelle)
```csharp
Point3dCollection controlPoints = new Point3dCollection();
controlPoints.Add(new Point3d(0, 0, 0));
controlPoints.Add(new Point3d(10, 10, 0));
controlPoints.Add(new Point3d(20, 10, 0));
controlPoints.Add(new Point3d(30, 0, 0));

// Définir les poids (poids plus élevé attire la courbe vers le point de contrôle)
DoubleCollection weights = new DoubleCollection();
weights.Add(1.0);  // Poids normal
weights.Add(2.0);  // Poids double - attire la courbe vers ce point
weights.Add(2.0);  // Poids double
weights.Add(1.0);  // Poids normal

DoubleCollection knots = new DoubleCollection();
knots.Add(0); knots.Add(0); knots.Add(0); knots.Add(0);
knots.Add(1); knots.Add(1); knots.Add(1); knots.Add(1);

// Créer la courbe NURBS rationnelle
NurbCurve3d rationalCurve = new NurbCurve3d(3, knots, controlPoints, weights, false);

ed.WriteMessage($"\nNURBS rationnelle : {rationalCurve.IsRational}");
ed.WriteMessage($"\nPoids : {rationalCurve.NumWeights}");
```

### Exemple 4: Modifier une Courbe NURBS
```csharp
NurbCurve3d curve = /* courbe existante */;

// Ajouter un point de contrôle
Point3d newControlPoint = new Point3d(15, 8, 0);
curve.AddControlPointAt(2, newControlPoint);

// Modifier un poids
if (curve.IsRational)
{
    curve.SetWeightAt(1, 3.0); // Augmenter le poids à l'index 1
}

// Insérer un nœud pour ajouter des détails
curve.InsertKnot(0.5);

// Élever le degré pour une courbe plus lisse
curve.ElevateDegree(4);

ed.WriteMessage($"\nCourbe modifiée :");
ed.WriteMessage($"\nPoints de contrôle : {curve.NumControlPoints}");
ed.WriteMessage($"\nDegré : {curve.Degree}");
```

### Exemple 5: Convertir NURBS en Polyligne
```csharp
NurbCurve3d nurbsCurve = /* courbe NURBS existante */;

// Échantillonner la courbe pour créer une approximation en polyligne
int numSegments = 50;
Point3dCollection polylinePoints = new Point3dCollection();

for (int i = 0; i <= numSegments; i++)
{
    double param = i / (double)numSegments;
    Point3d pt = nurbsCurve.EvaluatePoint(param);
    polylinePoints.Add(pt);
}

// Créer l'entité polyligne
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = new Polyline();
    pline.SetDatabaseDefaults();
    
    for (int i = 0; i < polylinePoints.Count; i++)
    {
        Point3d pt = polylinePoints[i];
        pline.AddVertexAt(i, new Point2d(pt.X, pt.Y), 0, 0, 0);
    }
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}

ed.WriteMessage($"\nNURBS convertie en polyligne avec {polylinePoints.Count} sommets");
```

### Exemple 6: Trouver le Point le Plus Proche sur NURBS
```csharp
NurbCurve3d curve = /* courbe NURBS existante */;
Point3d testPoint = new Point3d(15, 7, 0);

// Trouver le point le plus proche sur la courbe
Point3d closestPoint = curve.GetClosestPointTo(testPoint);
double distance = curve.GetDistanceTo(testPoint);

ed.WriteMessage($"\nPoint de test : {testPoint}");
ed.WriteMessage($"\nPoint le plus proche sur la courbe : {closestPoint}");
ed.WriteMessage($"\nDistance : {distance:F4}");

// Dessiner une ligne montrant le point le plus proche
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = new Line(testPoint, closestPoint);
    line.ColorIndex = 1; // Rouge
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    tr.Commit();
}
```

## Modèles Courants

### Créer un Vecteur de Nœuds Uniforme
```csharp
// Pour un degré d et n points de contrôle : nœuds = n + d + 1
int degree = 3;
int numControlPoints = 6;
int numKnots = numControlPoints + degree + 1; // 10 nœuds

DoubleCollection knots = new DoubleCollection();

// Vecteur de nœuds serré (la courbe passe par les premier et dernier points de contrôle)
for (int i = 0; i <= degree; i++)
    knots.Add(0.0); // d+1 zéros au début

for (int i = 1; i < numControlPoints - degree; i++)
    knots.Add(i / (double)(numControlPoints - degree));

for (int i = 0; i <= degree; i++)
    knots.Add(1.0); // d+1 uns à la fin
```

### Vérifier les Propriétés NURBS
```csharp
NurbCurve3d curve = /* courbe existante */;

if (curve.IsRational)
    ed.WriteMessage("\nLa courbe est rationnelle (a des poids)");
else
    ed.WriteMessage("\nLa courbe est non rationnelle");

if (curve.IsPeriodic)
    ed.WriteMessage("\nLa courbe est périodique (fermée)");
else
    ed.WriteMessage("\nLa courbe est ouverte");

ed.WriteMessage($"\nDegré : {curve.Degree}");
ed.WriteMessage($"\nPoints de contrôle : {curve.NumControlPoints}");
```

## Bonnes Pratiques

1. **Vecteur de Nœuds** : S'assurer que longueur du vecteur de nœuds = numPointsDeContrôle + degré + 1
2. **Courbes Serrées** : Utiliser des nœuds répétés au début/fin pour des courbes passant par les extrémités
3. **Poids** : Utiliser des poids > 1 pour attirer la courbe vers le point de contrôle, < 1 pour repousser
4. **Degré** : Degré plus élevé = courbe plus lisse, mais plus de calculs
5. **Courbes Périodiques** : Pour les courbes fermées, utiliser des NURBS périodiques avec début/fin correspondants
6. **Évaluation** : Échantillonner la courbe avec suffisamment de points pour une conversion précise en polyligne
7. **Performance** : Mettre en cache les points évalués si on évalue les mêmes paramètres de manière répétée

## Classes Associées
- **NurbCurve2d** - Courbe NURBS 2D
- **CubicSplineCurve3d** - Spline d'interpolation cubique
- **Polyline3d** - Approximation linéaire par morceaux
- **Point3d** - Points de contrôle et points évalués
- **Vector3d** - Vecteurs tangents et dérivées

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe NurbCurve3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_NurbCurve3d)
- [NURBS sur Wikipedia](https://fr.wikipedia.org/wiki/NURBS)
