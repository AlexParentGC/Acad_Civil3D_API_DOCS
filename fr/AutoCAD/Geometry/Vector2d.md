# Structure Vector2d

## Vue d'Ensemble
La structure `Vector2d` représente une direction et une magnitude dans l'espace 2D (plan XY). Elle est utilisée pour les calculs géométriques planaires, les transformations 2D et les opérations qui ne nécessitent pas de composante Z.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `X` | `double` | Obtient la composante X |
| `Y` | `double` | Obtient la composante Y |
| `Length` | `double` | Obtient la longueur (magnitude) du vecteur |
| `LengthSqrd` | `double` | Obtient la longueur au carré (plus rapide que Length) |
| `Angle` | `double` | Obtient l'angle depuis l'axe X (radians) |
| `XAxis` | `Vector2d` (static) | Obtient le vecteur unitaire (1, 0) |
| `YAxis` | `Vector2d` (static) | Obtient le vecteur unitaire (0, 1) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetNormal()` | `Vector2d` | Retourne le vecteur unitaire normalisé |
| `DotProduct(Vector2d)` | `double` | Calcule le produit scalaire avec un autre vecteur |
| `GetAngleTo(Vector2d)` | `double` | Obtient l'angle vers un autre vecteur (radians) |
| `TransformBy(Matrix2d)` | `Vector2d` | Transforme le vecteur par une matrice 2D |
| `RotateBy(double)` | `Vector2d` | Effectue une rotation du vecteur par un angle |
| `Negate()` | `Vector2d` | Retourne le vecteur inversé |
| `MultiplyBy(double)` | `Vector2d` | Multiplie le vecteur par un scalaire |
| `IsParallelTo(Vector2d)` | `bool` | Vérifie si parallèle à un autre vecteur |
| `IsPerpendicularTo(Vector2d)` | `bool` | Vérifie si perpendiculaire à un autre vecteur |
| `IsCodirectionalTo(Vector2d)` | `bool` | Vérifie si pointe dans la même direction |
| `IsEqualTo(Vector2d)` | `bool` | Vérifie l'égalité avec la tolérance par défaut |
| `IsEqualTo(Vector2d, Tolerance)` | `bool` | Vérifie l'égalité avec une tolérance personnalisée |

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

### Exemple 1: Créer et Utiliser des Vecteurs 2D
```csharp
// Créer des vecteurs 2D
Vector2d vec1 = new Vector2d(3, 4);
Vector2d vec2 = new Vector2d(1, 0);

// Obtenir les propriétés
double length = vec1.Length; // 5.0
double angle = vec1.Angle; // Angle depuis l'axe X en radians
double angleDegrees = angle * (180.0 / Math.PI);

// Normaliser
Vector2d unitVec = vec1.GetNormal(); // (0.6, 0.8)

// Utiliser les axes prédéfinis
Vector2d xAxis = Vector2d.XAxis; // (1, 0)
Vector2d yAxis = Vector2d.YAxis; // (0, 1)

ed.WriteMessage($"\nVecteur : ({vec1.X}, {vec1.Y})");
ed.WriteMessage($"\nLongueur : {length:F2}");
ed.WriteMessage($"\nAngle : {angleDegrees:F2}°");
ed.WriteMessage($"\nVecteur unitaire : ({unitVec.X:F2}, {unitVec.Y:F2})");
```

### Exemple 2: Arithmétique Vectorielle 2D
```csharp
Vector2d v1 = new Vector2d(5, 3);
Vector2d v2 = new Vector2d(2, 1);

// Addition
Vector2d sum = v1 + v2; // (7, 4)

// Soustraction
Vector2d diff = v1 - v2; // (3, 2)

// Opérations scalaires
Vector2d scaled = v1 * 2.0; // (10, 6)
Vector2d divided = v1 / 2.0; // (2.5, 1.5)

// Négation
Vector2d negated = v1.Negate(); // (-5, -3)

ed.WriteMessage($"\nSomme : ({sum.X}, {sum.Y})");
ed.WriteMessage($"\nDifférence : ({diff.X}, {diff.Y})");
ed.WriteMessage($"\nMis à l'échelle : ({scaled.X}, {scaled.Y})");
```

### Exemple 3: Calculs d'Angles
```csharp
Vector2d v1 = new Vector2d(1, 0); // Axe X
Vector2d v2 = new Vector2d(1, 1); // 45° de l'axe X
Vector2d v3 = new Vector2d(0, 1); // Axe Y

// Obtenir les angles
double angle12 = v1.GetAngleTo(v2); // π/4 (45°)
double angle13 = v1.GetAngleTo(v3); // π/2 (90°)

// Convertir en degrés
double degrees12 = angle12 * (180.0 / Math.PI);
double degrees13 = angle13 * (180.0 / Math.PI);

ed.WriteMessage($"\nAngle v1 vers v2 : {degrees12:F2}°");
ed.WriteMessage($"\nAngle v1 vers v3 : {degrees13:F2}°");

// Obtenir la propriété angle (depuis l'axe X)
double v2Angle = v2.Angle * (180.0 / Math.PI);
ed.WriteMessage($"\nAngle de v2 depuis l'axe X : {v2Angle:F2}°");
```

### Exemple 4: Rotation
```csharp
Vector2d vec = new Vector2d(10, 0);

// Rotation de 45° dans le sens antihoraire
double angle = Math.PI / 4;
Vector2d rotated = vec.RotateBy(angle);

// Rotation de 90° pour obtenir la perpendiculaire
Vector2d perpendicular = vec.RotateBy(Math.PI / 2);

ed.WriteMessage($"\nOriginal : ({vec.X:F2}, {vec.Y:F2})");
ed.WriteMessage($"\nRotation 45° : ({rotated.X:F2}, {rotated.Y:F2})");
ed.WriteMessage($"\nPerpendiculaire : ({perpendicular.X:F2}, {perpendicular.Y:F2})");

// Créer un motif circulaire
for (int i = 0; i < 8; i++)
{
    double ang = (i * 2 * Math.PI) / 8;
    Vector2d rotVec = vec.RotateBy(ang);
    ed.WriteMessage($"\nPoint {i} : ({rotVec.X:F2}, {rotVec.Y:F2})");
}
```

### Exemple 5: Produit Scalaire et Perpendicularité
```csharp
Vector2d v1 = new Vector2d(3, 4);
Vector2d v2 = new Vector2d(-4, 3); // Perpendiculaire à v1

// Produit scalaire
double dot = v1.DotProduct(v2); // Devrait être 0 pour perpendiculaire

// Vérifier les relations
bool isPerp = v1.IsPerpendicularTo(v2);
bool isParallel = v1.IsParallelTo(v2);

ed.WriteMessage($"\nProduit scalaire : {dot:F6}");
ed.WriteMessage($"\nPerpendiculaire : {isPerp}");
ed.WriteMessage($"\nParallèle : {isParallel}");

// Utilisation pratique : vérifier si les vecteurs sont perpendiculaires
Vector2d a = new Vector2d(1, 2);
Vector2d b = new Vector2d(-2, 1);
double dotAB = a.DotProduct(b);
ed.WriteMessage($"\nVecteurs perpendiculaires : {Math.Abs(dotAB) < 0.001}");
```

### Exemple 6: Projection 2D
```csharp
// Projeter le vecteur A sur le vecteur B
Vector2d vecA = new Vector2d(5, 3);
Vector2d vecB = new Vector2d(1, 0); // Axe X

// Formule de projection : proj_B(A) = (A · B / |B|²) * B
double dotProduct = vecA.DotProduct(vecB);
double lengthSqrd = vecB.LengthSqrd;
Vector2d projection = vecB * (dotProduct / lengthSqrd);

// Composante perpendiculaire
Vector2d perpComponent = vecA - projection;

ed.WriteMessage($"\nVecteur A : ({vecA.X}, {vecA.Y})");
ed.WriteMessage($"\nProjection sur l'axe X : ({projection.X:F2}, {projection.Y:F2})");
ed.WriteMessage($"\nComposante perpendiculaire : ({perpComponent.X:F2}, {perpComponent.Y:F2})");
```

### Exemple 7: Conversion entre 2D et 3D
```csharp
// 2D vers 3D
Vector2d vec2d = new Vector2d(5, 3);
Vector3d vec3d = new Vector3d(vec2d.X, vec2d.Y, 0);

ed.WriteMessage($"\nVecteur 2D : ({vec2d.X}, {vec2d.Y})");
ed.WriteMessage($"\nVecteur 3D : ({vec3d.X}, {vec3d.Y}, {vec3d.Z})");

// 3D vers 2D (ignorer Z)
Vector3d vec3dInput = new Vector3d(10, 20, 30);
Vector2d vec2dResult = new Vector2d(vec3dInput.X, vec3dInput.Y);

ed.WriteMessage($"\nEntrée 3D : ({vec3dInput.X}, {vec3dInput.Y}, {vec3dInput.Z})");
ed.WriteMessage($"\nRésultat 2D : ({vec2dResult.X}, {vec2dResult.Y})");
```

### Exemple 8: Créer un Décalage en 2D
```csharp
// Créer un décalage perpendiculaire à la direction
Point2d start = new Point2d(0, 0);
Point2d end = new Point2d(10, 0);
double offsetDist = 5.0;

// Obtenir le vecteur de direction
Vector2d direction = new Vector2d(end.X - start.X, end.Y - start.Y);

// Obtenir la perpendiculaire (rotation de 90°)
Vector2d perpendicular = direction.RotateBy(Math.PI / 2);
Vector2d offsetVector = perpendicular.GetNormal() * offsetDist;

// Calculer les points décalés
Point2d offsetStart = new Point2d(start.X + offsetVector.X, start.Y + offsetVector.Y);
Point2d offsetEnd = new Point2d(end.X + offsetVector.X, end.Y + offsetVector.Y);

ed.WriteMessage($"\nOriginal : ({start.X}, {start.Y}) à ({end.X}, {end.Y})");
ed.WriteMessage($"\nDécalage : ({offsetStart.X:F2}, {offsetStart.Y:F2}) à ({offsetEnd.X:F2}, {offsetEnd.Y:F2})");
```

## Modèles Courants

### Obtenir la Direction entre Deux Points 2D
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 10);

Vector2d direction = new Vector2d(pt2.X - pt1.X, pt2.Y - pt1.Y);
Vector2d unitDirection = direction.GetNormal();
double distance = direction.Length;
```

### Vérifier le Côté du Point (Gauche/Droite de la Ligne)
```csharp
Point2d lineStart = new Point2d(0, 0);
Point2d lineEnd = new Point2d(10, 0);
Point2d testPoint = new Point2d(5, 5);

Vector2d lineVec = new Vector2d(lineEnd.X - lineStart.X, lineEnd.Y - lineStart.Y);
Vector2d pointVec = new Vector2d(testPoint.X - lineStart.X, testPoint.Y - lineStart.Y);

// Produit vectoriel en 2D (composante Z)
double cross = lineVec.X * pointVec.Y - lineVec.Y * pointVec.X;

if (cross > 0)
    ed.WriteMessage("\nLe point est à gauche");
else if (cross < 0)
    ed.WriteMessage("\nLe point est à droite");
else
    ed.WriteMessage("\nLe point est sur la ligne");
```

### Créer un Vecteur Unitaire depuis un Angle
```csharp
double angleDegrees = 45;
double angleRadians = angleDegrees * (Math.PI / 180.0);

Vector2d unitVec = new Vector2d(Math.Cos(angleRadians), Math.Sin(angleRadians));

ed.WriteMessage($"\nVecteur unitaire à {angleDegrees}° : ({unitVec.X:F4}, {unitVec.Y:F4})");
```

## Bonnes Pratiques

1. **Utiliser pour Opérations 2D** : Préférer Vector2d à Vector3d lors du travail dans le plan XY
2. **Performance** : Les opérations Vector2d sont plus rapides que Vector3d pour la géométrie planaire
3. **Propriété Angle** : Utiliser la propriété `Angle` pour obtenir la direction du vecteur depuis l'axe X
4. **Normalisation** : Toujours normaliser quand seule la direction est nécessaire
5. **Tolérance** : Utiliser l'égalité basée sur la tolérance pour les comparaisons en virgule flottante
6. **Immutabilité** : Vector2d est une structure ; les opérations retournent de nouveaux vecteurs
7. **Conversion** : Conversion facile vers/depuis Vector3d au besoin

## Classes Associées
- **Vector3d** - Vecteur 3D avec composantes X, Y, Z
- **Point2d** - Point 2D dans l'espace
- **Matrix2d** - Matrice de transformation 2D
- **Line2d** - Ligne 2D non bornée
- **LineSegment2d** - Segment de ligne 2D borné
- **Tolerance** - Tolérance pour les comparaisons

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Vector2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Vector2d)
- [Namespace Geometry](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
