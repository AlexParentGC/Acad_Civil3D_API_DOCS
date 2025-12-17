# Structure Matrix3d

## Vue d'Ensemble
La structure `Matrix3d` représente une matrice de transformation 4x4 utilisée pour les transformations géométriques dans l'espace 3D. Elle est essentielle pour la translation, la rotation, la mise à l'échelle, la symétrie et les conversions de systèmes de coordonnées dans AutoCAD.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `CoordinateSystem3d` | `CoordinateSystem3d` | Obtient le système de coordonnées représenté par la matrice |
| `Inverse` | `Matrix3d` | Obtient la matrice inverse |
| `IsIdentity` | `bool` | Vérifie si la matrice est une matrice identité |
| `IsUniscaledOrtho` | `bool` | Vérifie si la matrice est orthogonale avec mise à l'échelle uniforme |
| `Norm` | `double` | Obtient la norme de la matrice |

## Méthodes de Fabrique Statiques

| Méthode | Description |
|---------|-------------|
| `Identity` | Retourne la matrice identité (aucune transformation) |
| `Displacement(Vector3d)` | Crée une matrice de translation |
| `Rotation(double, Vector3d, Point3d)` | Crée une matrice de rotation autour d'un axe |
| `Scaling(double, Point3d)` | Crée une matrice de mise à l'échelle uniforme |
| `Scaling(Vector3d, Point3d)` | Crée une matrice de mise à l'échelle non uniforme |
| `Mirroring(Plane)` | Crée une transformation de symétrie à travers un plan |
| `Mirroring(Point3d)` | Crée une transformation de symétrie à travers un point |
| `Mirroring(Line3d)` | Crée une transformation de symétrie à travers une ligne |
| `AlignCoordinateSystem(...)` | Crée une matrice d'alignement de système de coordonnées |
| `PlaneToWorld(Plane)` | Crée une transformation plan vers monde |
| `WorldToPlane(Plane)` | Crée une transformation monde vers plan |
| `Projection(Plane, Vector3d)` | Crée une matrice de projection sur un plan |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `PreMultiplyBy(Matrix3d)` | `Matrix3d` | Multiplie cette matrice par une autre (this * other) |
| `PostMultiplyBy(Matrix3d)` | `Matrix3d` | Multiplie une autre matrice par celle-ci (other * this) |
| `Transpose()` | `Matrix3d` | Retourne la matrice transposée |
| `Invert()` | `Matrix3d` | Retourne la matrice inversée |
| `GetCoordinateSystem()` | `CoordinateSystem3d` | Obtient le système de coordonnées de la matrice |
| `IsEqualTo(Matrix3d)` | `bool` | Vérifie l'égalité avec la tolérance par défaut |
| `IsEqualTo(Matrix3d, Tolerance)` | `bool` | Vérifie l'égalité avec une tolérance personnalisée |

## Exemples de Code

### Exemple 1: Translation (Déplacement)
```csharp
// Déplacer les entités de 10 unités en X, 5 en Y
Vector3d displacement = new Vector3d(10, 5, 0);
Matrix3d translation = Matrix3d.Displacement(displacement);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer un cercle
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    circle.SetDatabaseDefaults();
    
    // Appliquer la translation
    circle.TransformBy(translation);
    
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    tr.Commit();
}

ed.WriteMessage($"\nMatrice de translation créée : {displacement}");
ed.WriteMessage($"\nCercle déplacé vers : {circle.Center}");
```

### Exemple 2: Rotation
```csharp
// Rotation de 45° autour de l'axe Z à l'origine
double angle = Math.PI / 4; // 45 degrés
Vector3d axis = Vector3d.ZAxis;
Point3d basePoint = Point3d.Origin;

Matrix3d rotation = Matrix3d.Rotation(angle, axis, basePoint);

// Appliquer à un point
Point3d pt = new Point3d(10, 0, 0);
Point3d rotatedPt = pt.TransformBy(rotation);

ed.WriteMessage($"\nPoint original : ({pt.X:F2}, {pt.Y:F2}, {pt.Z:F2})");
ed.WriteMessage($"\nRotation 45° : ({rotatedPt.X:F2}, {rotatedPt.Y:F2}, {rotatedPt.Z:F2})");

// Rotation d'entité autour d'un point personnalisé
Point3d rotationCenter = new Point3d(5, 5, 0);
Matrix3d rotation90 = Matrix3d.Rotation(Math.PI / 2, Vector3d.ZAxis, rotationCenter);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir l'entité et effectuer la rotation
    Entity ent = tr.GetObject(selectedId, OpenMode.ForWrite) as Entity;
    ent.TransformBy(rotation90);
    tr.Commit();
}
```

### Exemple 3: Mise à l'Échelle
```csharp
// Mise à l'échelle uniforme (2x)
Point3d scaleBase = Point3d.Origin;
Matrix3d uniformScale = Matrix3d.Scaling(2.0, scaleBase);

// Mise à l'échelle non uniforme (2x en X, 3x en Y, 1x en Z)
Vector3d scaleFactors = new Vector3d(2.0, 3.0, 1.0);
Matrix3d nonUniformScale = Matrix3d.Scaling(scaleFactors, scaleBase);

// Appliquer à une entité
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = new Circle(new Point3d(10, 10, 0), Vector3d.ZAxis, 5.0);
    circle.SetDatabaseDefaults();
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    // Mettre à l'échelle le cercle
    circle.TransformBy(uniformScale);
    
    tr.Commit();
}

ed.WriteMessage($"\nFacteur de mise à l'échelle uniforme : 2.0");
ed.WriteMessage($"\nMise à l'échelle non uniforme : X={scaleFactors.X}, Y={scaleFactors.Y}, Z={scaleFactors.Z}");
```

### Exemple 4: Symétrie
```csharp
// Symétrie à travers le plan YZ (X = 0)
Point3d planePoint = Point3d.Origin;
Vector3d planeNormal = Vector3d.XAxis;
Plane mirrorPlane = new Plane(planePoint, planeNormal);

Matrix3d mirror = Matrix3d.Mirroring(mirrorPlane);

// Appliquer à un point
Point3d pt = new Point3d(10, 5, 0);
Point3d mirroredPt = pt.TransformBy(mirror);

ed.WriteMessage($"\nOriginal : ({pt.X}, {pt.Y}, {pt.Z})");
ed.WriteMessage($"\nSymétrie : ({mirroredPt.X}, {mirroredPt.Y}, {mirroredPt.Z})");

// Symétrie d'entité à travers une ligne personnalisée
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Définir la ligne de symétrie
    Point3d pt1 = new Point3d(0, 0, 0);
    Point3d pt2 = new Point3d(10, 10, 0);
    
    // Créer un plan perpendiculaire au XY à travers la ligne
    Vector3d lineVec = pt1.GetVectorTo(pt2);
    Vector3d perpVec = lineVec.CrossProduct(Vector3d.ZAxis).GetNormal();
    Plane plane = new Plane(pt1, perpVec);
    
    Matrix3d mirrorMatrix = Matrix3d.Mirroring(plane);
    
    // Appliquer à l'entité
    Entity ent = tr.GetObject(selectedId, OpenMode.ForWrite) as Entity;
    ent.TransformBy(mirrorMatrix);
    
    tr.Commit();
}
```

### Exemple 5: Combinaison de Transformations
```csharp
// Déplacer, puis rotation, puis mise à l'échelle
Vector3d moveVec = new Vector3d(10, 5, 0);
Matrix3d move = Matrix3d.Displacement(moveVec);

double rotAngle = Math.PI / 4;
Matrix3d rotate = Matrix3d.Rotation(rotAngle, Vector3d.ZAxis, Point3d.Origin);

double scaleFactor = 2.0;
Matrix3d scale = Matrix3d.Scaling(scaleFactor, Point3d.Origin);

// Combiner : l'ordre est important ! (scale * rotate * move)
Matrix3d combined = move.PreMultiplyBy(rotate).PreMultiplyBy(scale);

// Alternative utilisant PostMultiplyBy
Matrix3d combined2 = scale.PostMultiplyBy(rotate).PostMultiplyBy(move);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    circle.SetDatabaseDefaults();
    
    // Appliquer la transformation combinée
    circle.TransformBy(combined);
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}

ed.WriteMessage("\nTransformation combinée appliquée : Échelle -> Rotation -> Déplacement");
```

### Exemple 6: Alignement de Système de Coordonnées
```csharp
// Aligner d'un système de coordonnées à un autre
Point3d fromOrigin = Point3d.Origin;
Vector3d fromXAxis = Vector3d.XAxis;
Vector3d fromYAxis = Vector3d.YAxis;
Vector3d fromZAxis = Vector3d.ZAxis;

Point3d toOrigin = new Point3d(10, 10, 0);
Vector3d toXAxis = new Vector3d(0, 1, 0); // Rotation de 90°
Vector3d toYAxis = new Vector3d(-1, 0, 0);
Vector3d toZAxis = Vector3d.ZAxis;

Matrix3d alignment = Matrix3d.AlignCoordinateSystem(
    fromOrigin, fromXAxis, fromYAxis, fromZAxis,
    toOrigin, toXAxis, toYAxis, toZAxis
);

// Appliquer à une entité
Point3d testPt = new Point3d(5, 0, 0);
Point3d alignedPt = testPt.TransformBy(alignment);

ed.WriteMessage($"\nPoint original : {testPt}");
ed.WriteMessage($"\nPoint aligné : {alignedPt}");
```

### Exemple 7: Transformations de Plan
```csharp
// Transformer des coordonnées monde vers coordonnées de plan
Point3d planeOrigin = new Point3d(10, 10, 0);
Vector3d planeNormal = new Vector3d(0, 0, 1);
Plane workPlane = new Plane(planeOrigin, planeNormal);

// Monde vers plan
Matrix3d worldToPlane = Matrix3d.WorldToPlane(workPlane);

// Plan vers monde
Matrix3d planeToWorld = Matrix3d.PlaneToWorld(workPlane);

// Transformer un point de monde vers coordonnées de plan
Point3d worldPt = new Point3d(15, 15, 0);
Point3d planePt = worldPt.TransformBy(worldToPlane);

ed.WriteMessage($"\nPoint monde : {worldPt}");
ed.WriteMessage($"\nCoordonnées de plan : {planePt}");

// Transformer de retour
Point3d backToWorld = planePt.TransformBy(planeToWorld);
ed.WriteMessage($"\nRetour au monde : {backToWorld}");
```

### Exemple 8: Transformations Inverses
```csharp
// Créer une transformation
Vector3d displacement = new Vector3d(10, 5, 3);
Matrix3d transform = Matrix3d.Displacement(displacement);

// Obtenir l'inverse (annuler la transformation)
Matrix3d inverse = transform.Inverse;

// Appliquer la transformation puis l'inverse
Point3d original = new Point3d(5, 5, 5);
Point3d transformed = original.TransformBy(transform);
Point3d backToOriginal = transformed.TransformBy(inverse);

ed.WriteMessage($"\nOriginal : {original}");
ed.WriteMessage($"\nTransformé : {transformed}");
ed.WriteMessage($"\nRetour à l'original : {backToOriginal}");

// Vérifier qu'ils sont égaux
bool isEqual = original.IsEqualTo(backToOriginal);
ed.WriteMessage($"\nPoints égaux : {isEqual}");
```

### Exemple 9: Vérifier les Propriétés de Matrice
```csharp
Matrix3d identity = Matrix3d.Identity;
Matrix3d translation = Matrix3d.Displacement(new Vector3d(5, 5, 0));
Matrix3d rotation = Matrix3d.Rotation(Math.PI / 4, Vector3d.ZAxis, Point3d.Origin);

// Vérifier si identité
bool isIdentity1 = identity.IsIdentity; // true
bool isIdentity2 = translation.IsIdentity; // false

// Vérifier si orthogonale avec mise à l'échelle uniforme
bool isOrtho1 = rotation.IsUniscaledOrtho; // true (la rotation préserve les angles et longueurs)
bool isOrtho2 = translation.IsUniscaledOrtho; // true (la translation préserve tout)

ed.WriteMessage($"\nMatrice identité est identité : {isIdentity1}");
ed.WriteMessage($"\nTranslation est identité : {isIdentity2}");
ed.WriteMessage($"\nRotation est ortho avec échelle uniforme : {isOrtho1}");
ed.WriteMessage($"\nTranslation est ortho avec échelle uniforme : {isOrtho2}");

// Obtenir le système de coordonnées de la matrice
CoordinateSystem3d cs = rotation.CoordinateSystem3d;
ed.WriteMessage($"\nOrigine du système de coordonnées : {cs.Origin}");
```

### Exemple 10: Pratique - Copier et Transformer des Entités
```csharp
[CommandMethod("COPYROTATE")]
public void CopyAndRotate()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Sélectionner l'entité
    PromptEntityOptions peo = new PromptEntityOptions("\nSélectionner l'entité à copier et faire pivoter : ");
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    // Obtenir le point de base
    PromptPointOptions ppo = new PromptPointOptions("\nSpécifier le centre de rotation : ");
    PromptPointResult ppr = ed.GetPoint(ppo);
    if (ppr.Status != PromptStatus.OK) return;
    Point3d basePoint = ppr.Value;
    
    // Obtenir le nombre de copies
    PromptIntegerOptions pio = new PromptIntegerOptions("\nNombre de copies : ");
    pio.DefaultValue = 8;
    PromptIntegerResult pir = ed.GetInteger(pio);
    if (pir.Status != PromptStatus.OK) return;
    int numCopies = pir.Value;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Entity sourceEnt = tr.GetObject(per.ObjectId, OpenMode.ForRead) as Entity;
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
        
        double angleIncrement = (2 * Math.PI) / numCopies;
        
        for (int i = 1; i < numCopies; i++)
        {
            // Créer une copie
            Entity copy = sourceEnt.Clone() as Entity;
            
            // Créer la matrice de rotation
            double angle = angleIncrement * i;
            Matrix3d rotation = Matrix3d.Rotation(angle, Vector3d.ZAxis, basePoint);
            
            // Appliquer la transformation
            copy.TransformBy(rotation);
            
            // Ajouter à la base de données
            btr.AppendEntity(copy);
            tr.AddNewlyCreatedDBObject(copy, true);
        }
        
        tr.Commit();
        ed.WriteMessage($"\n{numCopies - 1} copies pivotées créées");
    }
}
```

## Modèles Courants

### Déplacer une Entité vers l'Origine
```csharp
Point3d currentPosition = entity.GeometricExtents.MinPoint;
Vector3d toOrigin = currentPosition.GetVectorTo(Point3d.Origin);
Matrix3d move = Matrix3d.Displacement(toOrigin);
entity.TransformBy(move);
```

### Rotation Autour du Centre de l'Entité
```csharp
Point3d center = entity.GeometricExtents.MinPoint + 
    ((entity.GeometricExtents.MaxPoint - entity.GeometricExtents.MinPoint) * 0.5);
Matrix3d rotation = Matrix3d.Rotation(angle, Vector3d.ZAxis, center);
entity.TransformBy(rotation);
```

### Mise à l'Échelle depuis le Centre de l'Entité
```csharp
Point3d center = entity.GeometricExtents.MinPoint + 
    ((entity.GeometricExtents.MaxPoint - entity.GeometricExtents.MinPoint) * 0.5);
Matrix3d scale = Matrix3d.Scaling(scaleFactor, center);
entity.TransformBy(scale);
```

## Ordre de Transformation

**CRITIQUE** : L'ordre de multiplication des matrices est important !

```csharp
// Utilisant PreMultiplyBy : s'applique de droite à gauche
Matrix3d result = move.PreMultiplyBy(rotate).PreMultiplyBy(scale);
// Équivalent à : scale * rotate * move
// L'entité est : déplacée, puis pivotée, puis mise à l'échelle

// Utilisant PostMultiplyBy : s'applique de gauche à droite
Matrix3d result = scale.PostMultiplyBy(rotate).PostMultiplyBy(move);
// Équivalent à : scale * rotate * move
// Même résultat que ci-dessus
```

## Bonnes Pratiques

1. **Ordre de Transformation** : Soyez attentif à l'ordre de transformation - il affecte le résultat
2. **Utiliser les Méthodes de Fabrique** : Utiliser les méthodes de fabrique statiques au lieu de construire les matrices manuellement
3. **Combiner les Transformations** : Combiner plusieurs transformations en une seule matrice pour l'efficacité
4. **Matrices Inverses** : Utiliser la propriété `Inverse` pour annuler les transformations
5. **Vérification d'Identité** : Utiliser `IsIdentity` pour vérifier si la transformation ne fait rien
6. **Systèmes de Coordonnées** : Utiliser `AlignCoordinateSystem` pour les conversions de coordonnées complexes
7. **Performance** : Appliquer la matrice combinée une fois plutôt que plusieurs transformations
8. **Immutabilité** : Matrix3d est une structure ; les opérations retournent de nouvelles matrices

## Classes Associées
- **Vector3d** - Direction et magnitude pour le déplacement
- **Point3d** - Points à transformer
- **Plane** - Plan pour la symétrie et les projections
- **CoordinateSystem3d** - Représentation du système de coordonnées
- **Entity** - Toutes les entités ont la méthode `TransformBy()`
- **Matrix2d** - Matrice de transformation 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Matrix3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Matrix3d)
- [Namespace Geometry](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
