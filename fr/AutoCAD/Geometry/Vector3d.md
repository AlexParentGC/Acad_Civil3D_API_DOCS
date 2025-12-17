# Structure Vector3d

## Vue d'Ensemble
La structure `Vector3d` représente une direction et une magnitude dans l'espace 3D. C'est une classe de géométrie fondamentale dans l'API .NET AutoCAD, utilisée extensivement pour les calculs directionnels, les transformations, les vecteurs normaux et les opérations géométriques.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `X` | `double` | Obtient la composante X |
| `Y` | `double` | Obtient la composante Y |
| `Z` | `double` | Obtient la composante Z |
| `Length` | `double` | Obtient la longueur (magnitude) du vecteur |
| `LengthSqrd` | `double` | Obtient la longueur au carré (plus rapide que Length) |
| `XAxis` | `Vector3d` (static) | Obtient le vecteur unitaire (1, 0, 0) |
| `YAxis` | `Vector3d` (static) | Obtient le vecteur unitaire (0, 1, 0) |
| `ZAxis` | `Vector3d` (static) | Obtient le vecteur unitaire (0, 0, 1) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetNormal()` | `Vector3d` | Retourne le vecteur unitaire normalisé |
| `DotProduct(Vector3d)` | `double` | Calcule le produit scalaire avec un autre vecteur |
| `CrossProduct(Vector3d)` | `Vector3d` | Calcule le produit vectoriel (vecteur perpendiculaire) |
| `GetAngleTo(Vector3d)` | `double` | Obtient l'angle vers un autre vecteur (radians) |
| `GetAngleTo(Vector3d, Vector3d)` | `double` | Obtient l'angle signé autour d'un vecteur de référence |
| `TransformBy(Matrix3d)` | `Vector3d` | Transforme le vecteur par une matrice |
| `RotateBy(double, Vector3d)` | `Vector3d` | Effectue une rotation du vecteur autour d'un axe |
| `Negate()` | `Vector3d` | Retourne le vecteur inversé |
| `MultiplyBy(double)` | `Vector3d` | Multiplie le vecteur par un scalaire |
| `IsParallelTo(Vector3d)` | `bool` | Vérifie si parallèle à un autre vecteur |
| `IsPerpendicularTo(Vector3d)` | `bool` | Vérifie si perpendiculaire à un autre vecteur |
| `IsCodirectionalTo(Vector3d)` | `bool` | Vérifie si pointe dans la même direction |
| `IsEqualTo(Vector3d)` | `bool` | Vérifie l'égalité avec la tolérance par défaut |
| `IsEqualTo(Vector3d, Tolerance)` | `bool` | Vérifie l'égalité avec une tolérance personnalisée |

## Surcharges d'Opérateurs

| Opérateur | Description |
|-----------|-------------|
| `+` | Addition de vecteurs |
| `-` | Soustraction de vecteurs |
| `*` | Multiplication scalaire |
| `/` | Division scalaire |
| `==` | Comparaison d'égalité |
| `!=` | Comparaison d'inégalité |

## Exemples de Code

### Exemple 1: Créer et Normaliser des Vecteurs
```csharp
// Créer des vecteurs
Vector3d vec1 = new Vector3d(3, 4, 0);
Vector3d vec2 = new Vector3d(10, 0, 0);

// Obtenir les propriétés du vecteur
double length = vec1.Length; // 5.0
double lengthSqrd = vec1.LengthSqrd; // 25.0 (calcul plus rapide)

// Normaliser en vecteur unitaire
Vector3d unitVec = vec1.GetNormal(); // (0.6, 0.8, 0)

// Utiliser les vecteurs d'axes prédéfinis
Vector3d xAxis = Vector3d.XAxis; // (1, 0, 0)
Vector3d yAxis = Vector3d.YAxis; // (0, 1, 0)
Vector3d zAxis = Vector3d.ZAxis; // (0, 0, 1)

ed.WriteMessage($"\nVecteur : ({vec1.X}, {vec1.Y}, {vec1.Z})");
ed.WriteMessage($"\nLongueur : {length:F2}");
ed.WriteMessage($"\nVecteur unitaire : ({unitVec.X:F2}, {unitVec.Y:F2}, {unitVec.Z:F2})");
```

### Exemple 2: Arithmétique Vectorielle
```csharp
Vector3d v1 = new Vector3d(1, 2, 3);
Vector3d v2 = new Vector3d(4, 5, 6);

// Addition
Vector3d sum = v1 + v2; // (5, 7, 9)

// Soustraction
Vector3d diff = v2 - v1; // (3, 3, 3)

// Multiplication scalaire
Vector3d scaled = v1 * 2.5; // (2.5, 5, 7.5)

// Division scalaire
Vector3d divided = v1 / 2.0; // (0.5, 1, 1.5)

// Négation
Vector3d negated = v1.Negate(); // (-1, -2, -3)

ed.WriteMessage($"\nSomme : {sum}");
ed.WriteMessage($"\nDifférence : {diff}");
ed.WriteMessage($"\nMis à l'échelle : {scaled}");
```

### Exemple 3: Produit Scalaire et Produit Vectoriel
```csharp
Vector3d v1 = new Vector3d(1, 0, 0);
Vector3d v2 = new Vector3d(0, 1, 0);

// Produit scalaire (résultat scalaire)
double dot = v1.DotProduct(v2); // 0 (vecteurs perpendiculaires)

// Produit vectoriel (résultat vectoriel - perpendiculaire aux deux)
Vector3d cross = v1.CrossProduct(v2); // (0, 0, 1) - Axe Z

// Exemple pratique : vérifier si les vecteurs sont perpendiculaires
Vector3d a = new Vector3d(3, 4, 0);
Vector3d b = new Vector3d(-4, 3, 0);
double dotAB = a.DotProduct(b); // 0 (perpendiculaire)

ed.WriteMessage($"\nProduit scalaire : {dot}");
ed.WriteMessage($"\nProduit vectoriel : ({cross.X}, {cross.Y}, {cross.Z})");
ed.WriteMessage($"\nVecteurs perpendiculaires : {Math.Abs(dotAB) < 0.001}");
```

### Exemple 4: Calculs d'Angles
```csharp
Vector3d v1 = new Vector3d(1, 0, 0); // Axe X
Vector3d v2 = new Vector3d(1, 1, 0); // 45° de l'axe X

// Obtenir l'angle entre les vecteurs (toujours positif, 0 à π)
double angle = v1.GetAngleTo(v2);
double degrees = angle * (180.0 / Math.PI); // Convertir en degrés

// Obtenir l'angle signé (avec vecteur de référence)
Vector3d refVec = Vector3d.ZAxis;
double signedAngle = v1.GetAngleTo(v2, refVec);

ed.WriteMessage($"\nAngle : {angle:F4} radians ({degrees:F2}°)");
ed.WriteMessage($"\nAngle signé : {signedAngle:F4} radians");

// Exemple pratique : trouver l'angle entre deux lignes
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(10, 10, 0);

Vector3d line1 = p1.GetVectorTo(p2);
Vector3d line2 = p2.GetVectorTo(p3);
double lineAngle = line1.GetAngleTo(line2);

ed.WriteMessage($"\nAngle entre les lignes : {lineAngle * (180.0 / Math.PI):F2}°");
```

### Exemple 5: Transformations de Vecteurs
```csharp
Vector3d vec = new Vector3d(1, 0, 0);

// Rotation du vecteur de 90° autour de l'axe Z
double angle = Math.PI / 2;
Vector3d rotated = vec.RotateBy(angle, Vector3d.ZAxis);
// Résultat : (0, 1, 0)

// Transformation par matrice
Matrix3d rotation = Matrix3d.Rotation(Math.PI / 4, Vector3d.ZAxis, Point3d.Origin);
Vector3d transformed = vec.TransformBy(rotation);

// Mise à l'échelle du vecteur
Vector3d scaled = vec.MultiplyBy(5.0); // (5, 0, 0)

ed.WriteMessage($"\nOriginal : {vec}");
ed.WriteMessage($"\nRotation 90° : ({rotated.X:F2}, {rotated.Y:F2}, {rotated.Z:F2})");
ed.WriteMessage($"\nTransformé : ({transformed.X:F2}, {transformed.Y:F2}, {transformed.Z:F2})");
```

### Exemple 6: Génération de Vecteur Perpendiculaire
```csharp
Vector3d vec = new Vector3d(1, 1, 0);

// Obtenir un vecteur perpendiculaire en utilisant le produit vectoriel
Vector3d perp1 = vec.CrossProduct(Vector3d.ZAxis);
perp1 = perp1.GetNormal(); // Normaliser

// Alternative : rotation de 90°
Vector3d perp2 = vec.RotateBy(Math.PI / 2, Vector3d.ZAxis);
perp2 = perp2.GetNormal();

// Vérifier la perpendicularité
double dot = vec.DotProduct(perp1);
bool isPerpendicular = Math.Abs(dot) < 0.001;

ed.WriteMessage($"\nVecteur original : {vec}");
ed.WriteMessage($"\nVecteur perpendiculaire : {perp1}");
ed.WriteMessage($"\nProduit scalaire (devrait être ~0) : {dot:F6}");
ed.WriteMessage($"\nEst perpendiculaire : {isPerpendicular}");
```

### Exemple 7: Projection de Vecteur
```csharp
// Projeter le vecteur A sur le vecteur B
Vector3d vecA = new Vector3d(5, 3, 0);
Vector3d vecB = new Vector3d(1, 0, 0); // Axe X

// Formule de projection : proj_B(A) = (A · B / |B|²) * B
double dotProduct = vecA.DotProduct(vecB);
double lengthSqrd = vecB.LengthSqrd;
Vector3d projection = vecB * (dotProduct / lengthSqrd);

// Obtenir la composante perpendiculaire (rejet)
Vector3d rejection = vecA - projection;

ed.WriteMessage($"\nVecteur A : {vecA}");
ed.WriteMessage($"\nVecteur B : {vecB}");
ed.WriteMessage($"\nProjection de A sur B : {projection}");
ed.WriteMessage($"\nRejet (perpendiculaire) : {rejection}");

// Vérifier : projection + rejet = original
Vector3d sum = projection + rejection;
ed.WriteMessage($"\nVérification (devrait égaler A) : {sum}");
```

### Exemple 8: Vérifier les Relations entre Vecteurs
```csharp
Vector3d v1 = new Vector3d(1, 0, 0);
Vector3d v2 = new Vector3d(2, 0, 0); // Parallèle à v1
Vector3d v3 = new Vector3d(0, 1, 0); // Perpendiculaire à v1
Vector3d v4 = new Vector3d(-1, 0, 0); // Opposé à v1

// Vérifier parallèle
bool isParallel = v1.IsParallelTo(v2); // true

// Vérifier perpendiculaire
bool isPerpendicular = v1.IsPerpendicularTo(v3); // true

// Vérifier codirectionnel (même direction)
bool isCodirectional = v1.IsCodirectionalTo(v2); // true
bool isCodirectional2 = v1.IsCodirectionalTo(v4); // false (opposé)

// Vérifier l'égalité avec tolérance
Tolerance tol = new Tolerance(0.001, 0.001);
Vector3d v5 = new Vector3d(1.0001, 0, 0);
bool isEqual = v1.IsEqualTo(v5, tol); // true

ed.WriteMessage($"\nv1 parallèle à v2 : {isParallel}");
ed.WriteMessage($"\nv1 perpendiculaire à v3 : {isPerpendicular}");
ed.WriteMessage($"\nv1 codirectionnel à v2 : {isCodirectional}");
ed.WriteMessage($"\nv1 codirectionnel à v4 : {isCodirectional2}");
```

### Exemple 9: Trouver l'Entité la Plus Proche d'une Coordonnée
```csharp
[CommandMethod("FINDNEAREST")]
public void FindNearestEntity()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Demander un point
    PromptPointResult ppr = ed.GetPoint("\nSélectionner le point de recherche : ");
    if (ppr.Status != PromptStatus.OK) return;
    
    Point3d searchPoint = ppr.Value;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
        
        Entity nearestEntity = null;
        double minDistance = double.MaxValue;
        Point3d nearestPoint = Point3d.Origin;
        
        foreach (ObjectId id in btr)
        {
            Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
            if (ent == null) continue;
            
            // Obtenir le point le plus proche sur l'entité
            Point3d closestPt = ent.GetClosestPointTo(searchPoint, false);
            
            // Calculer la distance en utilisant un vecteur
            Vector3d distanceVec = searchPoint.GetVectorTo(closestPt);
            double distance = distanceVec.Length;
            
            if (distance < minDistance)
            {
                minDistance = distance;
                nearestEntity = ent;
                nearestPoint = closestPt;
            }
        }
        
        if (nearestEntity != null)
        {
            // Calculer le vecteur de direction
            Vector3d direction = searchPoint.GetVectorTo(nearestPoint);
            Vector3d unitDirection = direction.GetNormal();
            
            ed.WriteMessage($"\n--- Entité la Plus Proche Trouvée ---");
            ed.WriteMessage($"\nType d'Entité : {nearestEntity.GetType().Name}");
            ed.WriteMessage($"\nDistance : {minDistance:F4}");
            ed.WriteMessage($"\nPoint le Plus Proche : ({nearestPoint.X:F2}, {nearestPoint.Y:F2}, {nearestPoint.Z:F2})");
            ed.WriteMessage($"\nVecteur de Direction : ({direction.X:F2}, {direction.Y:F2}, {direction.Z:F2})");
            ed.WriteMessage($"\nDirection Unitaire : ({unitDirection.X:F4}, {unitDirection.Y:F4}, {unitDirection.Z:F4})");
            
            // Mettre en surbrillance l'entité
            nearestEntity.Highlight();
            
            // Dessiner une ligne du point de recherche au point le plus proche
            Line indicator = new Line(searchPoint, nearestPoint);
            indicator.ColorIndex = 1; // Rouge
            
            BlockTableRecord space = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
            space.AppendEntity(indicator);
            tr.AddNewlyCreatedDBObject(indicator, true);
        }
        else
        {
            ed.WriteMessage("\nAucune entité trouvée dans l'espace courant.");
        }
        
        tr.Commit();
    }
}
```

### Exemple 10: Créer des Lignes Décalées en Utilisant des Vecteurs
```csharp
// Créer une ligne décalée perpendiculaire à l'originale
Point3d start = new Point3d(0, 0, 0);
Point3d end = new Point3d(10, 0, 0);
double offsetDistance = 5.0;

// Obtenir le vecteur de direction de la ligne
Vector3d lineDirection = start.GetVectorTo(end);

// Obtenir le vecteur perpendiculaire (rotation de 90° autour de Z)
Vector3d perpendicular = lineDirection.RotateBy(Math.PI / 2, Vector3d.ZAxis);
Vector3d offsetVector = perpendicular.GetNormal() * offsetDistance;

// Créer les points décalés
Point3d offsetStart = start.Add(offsetVector);
Point3d offsetEnd = end.Add(offsetVector);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Ligne originale
    Line originalLine = new Line(start, end);
    originalLine.ColorIndex = 7; // Blanc
    btr.AppendEntity(originalLine);
    tr.AddNewlyCreatedDBObject(originalLine, true);
    
    // Ligne décalée
    Line offsetLine = new Line(offsetStart, offsetEnd);
    offsetLine.ColorIndex = 3; // Vert
    btr.AppendEntity(offsetLine);
    tr.AddNewlyCreatedDBObject(offsetLine, true);
    
    tr.Commit();
}

ed.WriteMessage($"\nLigne originale : {start} à {end}");
ed.WriteMessage($"\nLigne décalée : {offsetStart} à {offsetEnd}");
ed.WriteMessage($"\nVecteur de décalage : {offsetVector}");
```

## Modèles Courants

### Obtenir la Direction entre Deux Points
```csharp
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 10, 0);

Vector3d direction = pt1.GetVectorTo(pt2);
Vector3d unitDirection = direction.GetNormal();
double distance = direction.Length;
```

### Créer un Vecteur Normal pour un Plan
```csharp
// Trois points définissent un plan
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(0, 10, 0);

Vector3d v1 = p1.GetVectorTo(p2);
Vector3d v2 = p1.GetVectorTo(p3);

// La normale est perpendiculaire aux deux vecteurs
Vector3d normal = v1.CrossProduct(v2).GetNormal();
```

### Vérifier si un Point est à Gauche ou à Droite d'une Ligne
```csharp
Point3d lineStart = new Point3d(0, 0, 0);
Point3d lineEnd = new Point3d(10, 0, 0);
Point3d testPoint = new Point3d(5, 5, 0);

Vector3d lineVec = lineStart.GetVectorTo(lineEnd);
Vector3d pointVec = lineStart.GetVectorTo(testPoint);

// La composante Z du produit vectoriel détermine le côté
Vector3d cross = lineVec.CrossProduct(pointVec);
if (cross.Z > 0)
    ed.WriteMessage("\nLe point est à gauche");
else if (cross.Z < 0)
    ed.WriteMessage("\nLe point est à droite");
else
    ed.WriteMessage("\nLe point est sur la ligne");
```

## Bonnes Pratiques

1. **Normalisation** : Toujours normaliser les vecteurs quand seule la direction est nécessaire : `vec.GetNormal()`
2. **Performance** : Utiliser `LengthSqrd` au lieu de `Length` lors de la comparaison de distances
3. **Immutabilité** : Vector3d est une structure ; les opérations retournent de nouveaux vecteurs
4. **Tolérance** : Utiliser des comparaisons basées sur la tolérance pour les vecteurs en virgule flottante
5. **Vecteurs Zéro** : Vérifier les vecteurs de longueur zéro avant de normaliser
6. **Produit Vectoriel** : Se rappeler que l'ordre du produit vectoriel importe : `A × B ≠ B × A`
7. **Produit Scalaire** : Utiliser pour les calculs d'angles et les projections
8. **Vecteurs Unitaires** : Utiliser les vecteurs prédéfinis `XAxis`, `YAxis`, `ZAxis` pour plus de clarté

## Classes Associées
- **Point3d** - Point 3D dans l'espace
- **Matrix3d** - Matrices de transformation
- **Plane** - Définition de plan utilisant un point et une normale
- **Vector2d** - Vecteur 2D pour les opérations planaires
- **Line3d** - Ligne 3D non bornée
- **Tolerance** - Tolérance pour les comparaisons

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Vector3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Vector3d)
- [Namespace Geometry](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
