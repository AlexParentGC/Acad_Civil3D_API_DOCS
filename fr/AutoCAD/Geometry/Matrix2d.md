# Structure Matrix2d

## Vue d'Ensemble
La structure `Matrix2d` représente une matrice de transformation 3x3 pour les transformations géométriques 2D. Elle est utilisée pour les opérations de translation, rotation, mise à l'échelle et symétrie dans le plan XY.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Inverse` | `Matrix2d` | Obtient la matrice inverse |
| `IsIdentity` | `bool` | Vérifie si la matrice est une matrice identité |
| `IsUniscaledOrtho` | `bool` | Vérifie si la matrice est orthogonale avec mise à l'échelle uniforme |

## Méthodes de Fabrique Statiques

| Méthode | Description |
|---------|-------------|
| `Identity` | Retourne la matrice identité (aucune transformation) |
| `Displacement(Vector2d)` | Crée une matrice de translation |
| `Rotation(double, Point2d)` | Crée une matrice de rotation autour d'un point |
| `Scaling(double, Point2d)` | Crée une matrice de mise à l'échelle uniforme |
| `Mirroring(Line2d)` | Crée une transformation de symétrie à travers une ligne |
| `Mirroring(Point2d)` | Crée une transformation de symétrie à travers un point |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `PreMultiplyBy(Matrix2d)` | `Matrix2d` | Multiplie cette matrice par une autre |
| `PostMultiplyBy(Matrix2d)` | `Matrix2d` | Multiplie une autre matrice par celle-ci |
| `Transpose()` | `Matrix2d` | Retourne la matrice transposée |
| `Invert()` | `Matrix2d` | Retourne la matrice inversée |
| `IsEqualTo(Matrix2d)` | `bool` | Vérifie l'égalité |

## Exemples de Code

### Exemple 1: Translation
```csharp
// Déplacer de 10 unités en X, 5 en Y
Vector2d displacement = new Vector2d(10, 5);
Matrix2d translation = Matrix2d.Displacement(displacement);

Point2d pt = new Point2d(0, 0);
Point2d moved = pt.TransformBy(translation);

ed.WriteMessage($"\nOriginal : ({pt.X}, {pt.Y})");
ed.WriteMessage($"\nDéplacé : ({moved.X}, {moved.Y})");
```

### Exemple 2: Rotation
```csharp
// Rotation de 45° autour de l'origine
double angle = Math.PI / 4;
Point2d center = new Point2d(0, 0);
Matrix2d rotation = Matrix2d.Rotation(angle, center);

Point2d pt = new Point2d(10, 0);
Point2d rotated = pt.TransformBy(rotation);

ed.WriteMessage($"\nRotation 45° : ({rotated.X:F2}, {rotated.Y:F2})");
```

### Exemple 3: Mise à l'Échelle
```csharp
// Mise à l'échelle 2x depuis l'origine
Point2d scaleBase = new Point2d(0, 0);
Matrix2d scale = Matrix2d.Scaling(2.0, scaleBase);

Point2d pt = new Point2d(5, 5);
Point2d scaled = pt.TransformBy(scale);

ed.WriteMessage($"\nMis à l'échelle : ({scaled.X}, {scaled.Y})"); // (10, 10)
```

### Exemple 4: Combinaison de Transformations
```csharp
// Déplacer, rotation, mise à l'échelle
Vector2d move = new Vector2d(10, 5);
Matrix2d translation = Matrix2d.Displacement(move);

Matrix2d rotation = Matrix2d.Rotation(Math.PI / 4, new Point2d(0, 0));
Matrix2d scale = Matrix2d.Scaling(2.0, new Point2d(0, 0));

// Combiner : scale * rotate * translate
Matrix2d combined = translation.PreMultiplyBy(rotation).PreMultiplyBy(scale);

Point2d pt = new Point2d(5, 0);
Point2d transformed = pt.TransformBy(combined);

ed.WriteMessage($"\nTransformé : ({transformed.X:F2}, {transformed.Y:F2})");
```

## Bonnes Pratiques

1. **Utiliser pour 2D** : Préférer Matrix2d à Matrix3d pour les transformations planaires
2. **Ordre de Transformation** : L'ordre de multiplication des matrices est important
3. **Combiner les Transformations** : Combiner plusieurs transformations pour l'efficacité
4. **Inverse** : Utiliser `Inverse` pour annuler les transformations

## Classes Associées
- **Matrix3d** - Matrice de transformation 3D
- **Vector2d** - Vecteurs de déplacement 2D
- **Point2d** - Points 2D à transformer
- **Line2d** - Ligne 2D pour la symétrie

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Matrix2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Matrix2d)
