# Classe Ray3d

## Vue d'Ensemble
La classe `Ray3d` représente une ligne semi-bornée dans l'espace 3D, définie par un point de base et un vecteur de direction. Contrairement à `Line3d` (non bornée dans les deux directions), un rayon s'étend à l'infini dans une seule direction à partir de son point de base.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `BasePoint` | `Point3d` | Obtient le point de départ du rayon |
| `Direction` | `Vector3d` | Obtient le vecteur de direction du rayon |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur le rayon vers un point donné |
| `DistanceTo(Point3d)` | `double` | Obtient la distance d'un point au rayon |
| `IsOn(Point3d)` | `bool` | Vérifie si un point est sur le rayon |
| `IntersectWith(...)` | `Point3d` | Obtient l'intersection avec un autre objet géométrique |

## Exemples de Code

### Exemple 1: Créer des Rayons
```csharp
// Rayon depuis l'origine pointant le long de l'axe X positif
Point3d basePoint = Point3d.Origin;
Vector3d direction = Vector3d.XAxis;

Ray3d ray = new Ray3d(basePoint, direction);

ed.WriteMessage($"\nRayon créé");
ed.WriteMessage($"\nPoint de base : {ray.BasePoint}");
ed.WriteMessage($"\nDirection : {ray.Direction}");
```

### Exemple 2: Lancer de Rayon
```csharp
// Lancer un rayon depuis la position de la caméra vers la cible
Point3d cameraPos = new Point3d(0, 0, 10);
Point3d target = new Point3d(5, 5, 0);

Vector3d direction = cameraPos.GetVectorTo(target).GetNormal();
Ray3d ray = new Ray3d(cameraPos, direction);

// Vérifier si le rayon touche un point
Point3d testPoint = new Point3d(2.5, 2.5, 5);
double distance = ray.DistanceTo(testPoint);

ed.WriteMessage($"\nRayon de la caméra vers la cible");
ed.WriteMessage($"\nDistance au point de test : {distance:F4}");
```

### Exemple 3: Trouver le Point le Plus Proche sur le Rayon
```csharp
Ray3d ray = new Ray3d(Point3d.Origin, Vector3d.XAxis);
Point3d testPoint = new Point3d(10, 5, 0);

Point3d closestPoint = ray.GetClosestPointTo(testPoint);
double distance = ray.DistanceTo(testPoint);

ed.WriteMessage($"\nPoint de test : {testPoint}");
ed.WriteMessage($"\nPoint le plus proche sur le rayon : {closestPoint}");
ed.WriteMessage($"\nDistance : {distance:F4}");
```

## Bonnes Pratiques

1. **Direction** : Doit être normalisée (vecteur unitaire)
2. **Semi-Borné** : S'étend à l'infini depuis le point de base dans la direction uniquement
3. **Lancer de Rayon** : Couramment utilisé pour la détection de collision et la sélection
4. **Point le Plus Proche** : Peut être le point de base si le point de test est "derrière" le rayon

## Classes Associées
- **Ray2d** - Ligne semi-bornée 2D
- **Line3d** - Ligne 3D non bornée
- **LineSegment3d** - Segment de ligne 3D borné
- **Point3d** - Point de base
- **Vector3d** - Vecteur de direction

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Ray3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Ray3d)
