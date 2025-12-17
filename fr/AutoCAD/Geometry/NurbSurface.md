# Classe NurbSurface

## Vue d'Ensemble
La classe `NurbSurface` représente une surface paramétrique NURB (B-spline rationnelle non uniforme) générique dans l'espace 3D. Les surfaces NURBS sont l'extension 3D des courbes NURBS, fournissant une représentation mathématique précise de surfaces complexes à forme libre.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ControlPoints` | `Point3d[,]` | Obtient la grille 2D de points de contrôle |
| `UKnots` | `KnotCollection` | Obtient le vecteur de nœuds dans la direction U |
| `VKnots` | `KnotCollection` | Obtient le vecteur de nœuds dans la direction V |
| `UWeights` | `DoubleCollection` | Obtient les poids dans la direction U |
| `VWeights` | `DoubleCollection` | Obtient les poids dans la direction V |
| `UDegree` | `int` | Obtient le degré polynomial dans la direction U |
| `VDegree` | `int` | Obtient le degré polynomial dans la direction V |
| `NumControlPointsInU` | `int` | Obtient le nombre de points de contrôle en U |
| `NumControlPointsInV` | `int` | Obtient le nombre de points de contrôle en V |
| `IsRational` | `bool` | Vérifie si la surface a des poids |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `EvaluatePoint(double, double)` | `Point3d` | Évalue un point aux paramètres (u,v) |
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la surface |
| `GetNormalAt(double, double)` | `Vector3d` | Obtient la normale de surface à (u,v) |
| `GetDerivativesAt(double, double)` | `Vector3d[]` | Obtient les dérivées partielles à (u,v) |

## Exemples de Code

### Exemple 1: Créer une Surface NURBS
```csharp
// Créer une grille de points de contrôle 4x4
int uCount = 4;
int vCount = 4;
Point3d[,] controlPoints = new Point3d[uCount, vCount];

for (int i = 0; i < uCount; i++)
{
    for (int j = 0; j < vCount; j++)
    {
        controlPoints[i, j] = new Point3d(i * 10, j * 10, Math.Sin(i) * Math.Cos(j) * 5);
    }
}

// Définir les vecteurs de nœuds (degré 3)
DoubleCollection uKnots = new DoubleCollection();
uKnots.Add(0); uKnots.Add(0); uKnots.Add(0); uKnots.Add(0);
uKnots.Add(1); uKnots.Add(1); uKnots.Add(1); uKnots.Add(1);

DoubleCollection vKnots = new DoubleCollection();
vKnots.Add(0); vKnots.Add(0); vKnots.Add(0); vKnots.Add(0);
vKnots.Add(1); vKnots.Add(1); vKnots.Add(1); vKnots.Add(1);

NurbSurface surface = new NurbSurface(3, 3, uKnots, vKnots, controlPoints, false);

ed.WriteMessage($"\nSurface NURBS créée");
ed.WriteMessage($"\nDegré U : {surface.UDegree}, Degré V : {surface.VDegree}");
ed.WriteMessage($"\nPoints de contrôle : {surface.NumControlPointsInU} x {surface.NumControlPointsInV}");
```

### Exemple 2: Évaluer des Points sur la Surface
```csharp
NurbSurface surface = /* surface NURBS existante */;

// Échantillonner la surface
for (double u = 0; u <= 1.0; u += 0.25)
{
    for (double v = 0; v <= 1.0; v += 0.25)
    {
        Point3d pt = surface.EvaluatePoint(u, v);
        Vector3d normal = surface.GetNormalAt(u, v);
        
        ed.WriteMessage($"\nÀ (u={u:F2}, v={v:F2}) : Point={pt}, Normale={normal}");
    }
}
```

### Exemple 3: Trouver le Point le Plus Proche sur la Surface
```csharp
NurbSurface surface = /* surface existante */;
Point3d testPoint = new Point3d(15, 15, 10);

Point3d closestPoint = surface.GetClosestPointTo(testPoint);
double distance = testPoint.DistanceTo(closestPoint);

ed.WriteMessage($"\nPoint de test : {testPoint}");
ed.WriteMessage($"\nPoint le plus proche sur la surface : {closestPoint}");
ed.WriteMessage($"\nDistance : {distance:F4}");
```

## Bonnes Pratiques

1. **Grille de Points de Contrôle** : Doit être rectangulaire (grille m x n)
2. **Vecteurs de Nœuds** : Longueur nœuds U = numU + degréU + 1, Longueur nœuds V = numV + degréV + 1
3. **Paramètres** : Les paramètres U et V vont généralement de 0 à 1
4. **Normales** : La normale de surface pointe perpendiculairement à la surface
5. **Rationnelle** : Utiliser des poids pour plus de contrôle sur la forme de la surface
6. **Évaluation** : Échantillonner la surface avec suffisamment de points (u,v) pour la visualisation

## Classes Associées
- **NurbCurve3d** - Courbe NURBS (1D)
- **BoundedPlane** - Surface planaire
- **Point3d** - Points de contrôle et points évalués
- **Vector3d** - Normales de surface

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe NurbSurface](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_NurbSurface)
- [Surfaces NURBS sur Wikipedia](https://fr.wikipedia.org/wiki/NURBS)
