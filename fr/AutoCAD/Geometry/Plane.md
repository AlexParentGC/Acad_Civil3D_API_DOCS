# Structure Plane

## Vue d'Ensemble
La structure `Plane` représente un plan infini dans l'espace 3D, défini par un point sur le plan et un vecteur normal perpendiculaire à celui-ci. Les plans sont utilisés pour la symétrie, les projections, les définitions de systèmes de coordonnées et les calculs géométriques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Normal` | `Vector3d` | Obtient le vecteur normal perpendiculaire au plan |
| `PointOnPlane` | `Point3d` | Obtient un point sur le plan |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `Plane(Point3d, Vector3d)` | Crée un plan depuis un point et un vecteur normal |
| `Plane(Point3d, Point3d, Point3d)` | Crée un plan depuis trois points non colinéaires |
| `Plane()` | Crée le plan XY à l'origine |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur le plan vers un point donné |
| `DistanceTo(Point3d)` | `double` | Obtient la distance signée d'un point au plan |
| `IsCoplanarTo(Plane)` | `bool` | Vérifie si coplanaire avec un autre plan |
| `IsPerpendicular(Plane)` | `bool` | Vérifie si perpendiculaire à un autre plan |
| `IsParallelTo(Plane)` | `bool` | Vérifie si parallèle à un autre plan |
| `IsEqualTo(Plane)` | `bool` | Vérifie l'égalité avec la tolérance par défaut |
| `IsEqualTo(Plane, Tolerance)` | `bool` | Vérifie l'égalité avec une tolérance personnalisée |

## Exemples de Code

### Exemple 1: Créer des Plans
```csharp
// Plan XY à l'origine (Z = 0)
Plane xyPlane = new Plane();

// Plan depuis un point et une normale
Point3d origin = Point3d.Origin;
Vector3d normal = Vector3d.ZAxis;
Plane customPlane = new Plane(origin, normal);

// Plan depuis trois points
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(0, 10, 0);
Plane planeFrom3Pts = new Plane(p1, p2, p3);

ed.WriteMessage($"\nNormale du plan XY : {xyPlane.Normal}");
ed.WriteMessage($"\nPoint du plan personnalisé : {customPlane.PointOnPlane}");
ed.WriteMessage($"\nNormale du plan 3 points : {planeFrom3Pts.Normal}");
```

### Exemple 2: Distance Point-Plan
```csharp
// Créer le plan XY à Z = 0
Plane xyPlane = new Plane(Point3d.Origin, Vector3d.ZAxis);

// Points de test
Point3d pt1 = new Point3d(5, 5, 10); // Au-dessus du plan
Point3d pt2 = new Point3d(3, 3, -5); // En dessous du plan
Point3d pt3 = new Point3d(7, 7, 0); // Sur le plan

// Calculer les distances signées
double dist1 = xyPlane.DistanceTo(pt1); // +10
double dist2 = xyPlane.DistanceTo(pt2); // -5
double dist3 = xyPlane.DistanceTo(pt3); // 0

ed.WriteMessage($"\nDistance pt1 au plan : {dist1:F2}");
ed.WriteMessage($"\nDistance pt2 au plan : {dist2:F2}");
ed.WriteMessage($"\nDistance pt3 au plan : {dist3:F2}");

// Positif = au-dessus du plan (dans la direction de la normale)
// Négatif = en dessous du plan (opposé à la normale)
// Zéro = sur le plan
```

### Exemple 3: Projeter des Points sur le Plan
```csharp
// Créer un plan vertical (plan YZ à X = 0)
Plane yzPlane = new Plane(Point3d.Origin, Vector3d.XAxis);

// Point à projeter
Point3d testPoint = new Point3d(10, 5, 3);

// Obtenir le point le plus proche sur le plan (projection)
Point3d projected = yzPlane.GetClosestPointTo(testPoint);

// Calculer la distance de projection
double distance = yzPlane.DistanceTo(testPoint);

ed.WriteMessage($"\nPoint original : {testPoint}");
ed.WriteMessage($"\nPoint projeté : {projected}");
ed.WriteMessage($"\nDistance de projection : {distance:F2}");

// Dessiner une ligne montrant la projection
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line projLine = new Line(testPoint, projected);
    projLine.ColorIndex = 1; // Rouge
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(projLine);
    tr.AddNewlyCreatedDBObject(projLine, true);
    tr.Commit();
}
```

### Exemple 4: Symétrie à Travers un Plan
```csharp
// Créer un plan de symétrie (plan YZ)
Plane mirrorPlane = new Plane(Point3d.Origin, Vector3d.XAxis);

// Créer une transformation de symétrie
Matrix3d mirror = Matrix3d.Mirroring(mirrorPlane);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Sélectionner l'entité à symétriser
    Entity ent = tr.GetObject(selectedId, OpenMode.ForWrite) as Entity;
    
    // Appliquer la transformation de symétrie
    ent.TransformBy(mirror);
    
    tr.Commit();
}

ed.WriteMessage($"\nEntité symétrisée à travers le plan");
ed.WriteMessage($"\nNormale du plan : {mirrorPlane.Normal}");
ed.WriteMessage($"\nPoint du plan : {mirrorPlane.PointOnPlane}");
```

### Exemple 5: Vérifier les Relations entre Plans
```csharp
// Créer des plans
Plane xyPlane = new Plane(Point3d.Origin, Vector3d.ZAxis);
Plane parallelPlane = new Plane(new Point3d(0, 0, 10), Vector3d.ZAxis);
Plane perpPlane = new Plane(Point3d.Origin, Vector3d.XAxis);

// Vérifier les relations
bool isParallel = xyPlane.IsParallelTo(parallelPlane); // true
bool isPerpendicular = xyPlane.IsPerpendicular(perpPlane); // true
bool isCoplanar = xyPlane.IsCoplanarTo(parallelPlane); // false

ed.WriteMessage($"\nXY parallèle à XY décalé : {isParallel}");
ed.WriteMessage($"\nXY perpendiculaire à YZ : {isPerpendicular}");
ed.WriteMessage($"\nXY coplanaire à XY décalé : {isCoplanar}");

// Vérifier si même plan
Plane samePlane = new Plane(new Point3d(5, 5, 0), Vector3d.ZAxis);
bool isCoplanar2 = xyPlane.IsCoplanarTo(samePlane); // true (même plan)

ed.WriteMessage($"\nXY coplanaire au même plan : {isCoplanar2}");
```

### Exemple 6: Créer un Plan depuis une Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir une entité planaire (ex: Circle, Region, Hatch)
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    // Créer un plan depuis les propriétés du cercle
    Plane circlePlane = new Plane(circle.Center, circle.Normal);
    
    ed.WriteMessage($"\nCentre du cercle : {circle.Center}");
    ed.WriteMessage($"\nNormale du cercle : {circle.Normal}");
    ed.WriteMessage($"\nPlan créé depuis le cercle");
    
    // Vérifier si un point est sur le plan du cercle
    Point3d testPt = new Point3d(10, 10, 0);
    double dist = circlePlane.DistanceTo(testPt);
    bool isOnPlane = Math.Abs(dist) < 0.001;
    
    ed.WriteMessage($"\nPoint de test sur le plan : {isOnPlane}");
    
    tr.Commit();
}
```

### Exemple 7: Plan depuis la Normale d'une Face
```csharp
// Créer un plan depuis trois points définissant une face
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(5, 10, 5);

// Calculer la normale en utilisant le produit vectoriel
Vector3d v1 = p1.GetVectorTo(p2);
Vector3d v2 = p1.GetVectorTo(p3);
Vector3d normal = v1.CrossProduct(v2).GetNormal();

// Créer le plan
Plane facePlane = new Plane(p1, normal);

// Alternative : utiliser le constructeur 3 points
Plane facePlane2 = new Plane(p1, p2, p3);

ed.WriteMessage($"\nPlan depuis vecteurs - Normale : {facePlane.Normal}");
ed.WriteMessage($"\nPlan depuis 3 points - Normale : {facePlane2.Normal}");
```

### Exemple 8: Vérifier le Côté d'un Point par Rapport au Plan
```csharp
// Créer un plan horizontal à Z = 5
Plane plane = new Plane(new Point3d(0, 0, 5), Vector3d.ZAxis);

Point3d above = new Point3d(0, 0, 10);
Point3d below = new Point3d(0, 0, 2);
Point3d onPlane = new Point3d(5, 5, 5);

// Vérifier de quel côté du plan
double distAbove = plane.DistanceTo(above);
double distBelow = plane.DistanceTo(below);
double distOn = plane.DistanceTo(onPlane);

if (distAbove > 0.001)
    ed.WriteMessage("\nLe point est au-dessus du plan (dans la direction de la normale)");
else if (distAbove < -0.001)
    ed.WriteMessage("\nLe point est en dessous du plan");
else
    ed.WriteMessage("\nLe point est sur le plan");

ed.WriteMessage($"\nDistances : au-dessus={distAbove:F2}, en dessous={distBelow:F2}, sur={distOn:F6}");
```

## Modèles Courants

### Créer un Plan de Construction
```csharp
// Obtenir le plan UCS
Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
Matrix3d ucs = ed.CurrentUserCoordinateSystem;
CoordinateSystem3d ucsCS = ucs.CoordinateSystem3d;

Plane ucsPlane = new Plane(ucsCS.Origin, ucsCS.Zaxis);
```

### Vérifier si des Points sont Coplanaires
```csharp
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(0, 10, 0);
Point3d p4 = new Point3d(5, 5, 0);

// Créer un plan depuis les trois premiers points
Plane plane = new Plane(p1, p2, p3);

// Vérifier si le quatrième point est sur le plan
double distance = plane.DistanceTo(p4);
bool isCoplanar = Math.Abs(distance) < 0.001;

ed.WriteMessage($"\nLes points sont coplanaires : {isCoplanar}");
```

### Obtenir un Plan Perpendiculaire
```csharp
// Plan original (XY)
Plane xyPlane = new Plane(Point3d.Origin, Vector3d.ZAxis);

// Créer un plan perpendiculaire passant par l'axe X
Plane perpPlane = new Plane(Point3d.Origin, Vector3d.YAxis);

// Vérifier la perpendicularité
bool isPerp = xyPlane.IsPerpendicular(perpPlane);
ed.WriteMessage($"\nPlans perpendiculaires : {isPerp}");
```

## Bonnes Pratiques

1. **Direction de la Normale** : S'assurer que le vecteur normal est normalisé pour des résultats cohérents
2. **Trois Points** : Lors de la création depuis 3 points, s'assurer qu'ils ne sont pas colinéaires
3. **Distance Signée** : Se rappeler que la distance est signée (positive/négative selon la normale)
4. **Tolérance** : Utiliser la tolérance lors de la vérification si des points sont sur le plan
5. **Systèmes de Coordonnées** : Les plans sont fondamentaux pour les UCS et les transformations de coordonnées
6. **Symétrie** : Utiliser les plans pour les transformations de symétrie
7. **Projections** : Utiliser `GetClosestPointTo()` pour les projections point-plan

## Classes Associées
- **Vector3d** - Vecteur normal pour la définition du plan
- **Point3d** - Point sur le plan
- **Matrix3d** - Transformations de plan (symétrie, projection)
- **Line3d** - Intersections ligne-plan
- **CoordinateSystem3d** - Définition du système de coordonnées

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Plane](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Plane)
- [Namespace Geometry](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
