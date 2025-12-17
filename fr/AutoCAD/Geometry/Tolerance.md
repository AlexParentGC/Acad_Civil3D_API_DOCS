# Structure Tolerance

## Vue d'Ensemble
La structure `Tolerance` définit des valeurs de tolérance pour les comparaisons géométriques dans AutoCAD. Elle fournit des tests d'égalité approximative pour les points, vecteurs et autres entités géométriques afin de tenir compte des problèmes de précision en virgule flottante.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `EqualPoint` | `double` | Tolérance pour les comparaisons d'égalité de points |
| `EqualVector` | `double` | Tolérance pour les comparaisons d'égalité de vecteurs |
| `Global` | `Tolerance` (static) | Obtient la tolérance globale par défaut |

## Constructeurs

| Constructeur | Description |
|--------------|-------------|
| `Tolerance(double, double)` | Crée une tolérance avec des tolérances de point et de vecteur |

## Exemples de Code

### Exemple 1: Utiliser la Tolérance pour la Comparaison de Points
```csharp
Point3d pt1 = new Point3d(10.0, 20.0, 0.0);
Point3d pt2 = new Point3d(10.0001, 20.0001, 0.0);

// Sans tolérance (comparaison exacte)
bool exactEqual = pt1 == pt2; // false

// Avec tolérance
Tolerance tol = new Tolerance(0.001, 0.001);
bool fuzzyEqual = pt1.IsEqualTo(pt2, tol); // true

ed.WriteMessage($"\nÉgalité exacte : {exactEqual}");
ed.WriteMessage($"\nÉgalité approximative (tol=0.001) : {fuzzyEqual}");
```

### Exemple 2: Utiliser la Tolérance Globale
```csharp
// Utiliser les paramètres de tolérance globale d'AutoCAD
Tolerance globalTol = Tolerance.Global;

Point3d pt1 = new Point3d(5.0, 5.0, 0.0);
Point3d pt2 = new Point3d(5.00001, 5.00001, 0.0);

bool isEqual = pt1.IsEqualTo(pt2, globalTol);

ed.WriteMessage($"\nTolérance de point globale : {globalTol.EqualPoint}");
ed.WriteMessage($"\nTolérance de vecteur globale : {globalTol.EqualVector}");
ed.WriteMessage($"\nPoints égaux avec tolérance globale : {isEqual}");
```

### Exemple 3: Comparaison de Vecteurs avec Tolérance
```csharp
Vector3d v1 = new Vector3d(1.0, 0.0, 0.0);
Vector3d v2 = new Vector3d(1.00001, 0.00001, 0.0);

Tolerance tol = new Tolerance(0.0001, 0.0001);
bool areEqual = v1.IsEqualTo(v2, tol);

ed.WriteMessage($"\nVecteurs égaux : {areEqual}");
```

### Exemple 4: Vérifier si un Point est sur une Ligne
```csharp
Point3d lineStart = new Point3d(0, 0, 0);
Point3d lineEnd = new Point3d(10, 0, 0);
Point3d testPoint = new Point3d(5.00001, 0.00001, 0);

// Calculer la distance à la ligne
Vector3d lineVec = lineStart.GetVectorTo(lineEnd);
Vector3d pointVec = lineStart.GetVectorTo(testPoint);
Vector3d cross = lineVec.CrossProduct(pointVec);
double distance = cross.Length / lineVec.Length;

// Vérifier avec tolérance
Tolerance tol = new Tolerance(0.001, 0.001);
bool isOnLine = distance < tol.EqualPoint;

ed.WriteMessage($"\nDistance à la ligne : {distance:F6}");
ed.WriteMessage($"\nPoint sur la ligne (dans la tolérance) : {isOnLine}");
```

## Bonnes Pratiques

1. **Utiliser la Tolérance** : Toujours utiliser la tolérance pour les comparaisons géométriques en virgule flottante
2. **Tolérance Globale** : Utiliser `Tolerance.Global` pour la cohérence avec les paramètres AutoCAD
3. **Valeurs Appropriées** : Choisir des valeurs de tolérance appropriées pour l'échelle de votre application
4. **Point vs Vecteur** : Utiliser des tolérances différentes pour les points et les vecteurs si nécessaire

## Classes Associées
- **Point3d** - Utilise la tolérance dans la méthode `IsEqualTo()`
- **Vector3d** - Utilise la tolérance dans la méthode `IsEqualTo()`
- **Point2d** - Utilise la tolérance pour les comparaisons 2D
- **Vector2d** - Utilise la tolérance pour les comparaisons de vecteurs 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Tolerance](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Tolerance)
