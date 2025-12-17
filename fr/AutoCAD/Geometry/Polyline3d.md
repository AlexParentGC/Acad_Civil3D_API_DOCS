# Classe Polyline3d

## Vue d'Ensemble
La classe `Polyline3d` représente une entité spline linéaire par morceaux dans l'espace 3D. Contrairement aux splines lisses, les polylignes consistent en segments de ligne droite reliant des sommets.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Vertices` | `Point3dCollection` | Obtient les sommets de la polyligne |
| `NumVertices` | `int` | Obtient le nombre de sommets |
| `IsClosed` | `bool` | Vérifie si la polyligne est fermée |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Obtient le point le plus proche sur la polyligne |
| `GetSegmentAt(int)` | `LineSegment3d` | Obtient le segment de ligne à l'index |
| `Length` | `double` | Obtient la longueur totale de la polyligne |

## Exemples de Code

### Exemple 1: Créer une Polyligne 3D
```csharp
Point3dCollection vertices = new Point3dCollection();
vertices.Add(new Point3d(0, 0, 0));
vertices.Add(new Point3d(10, 0, 0));
vertices.Add(new Point3d(10, 10, 0));
vertices.Add(new Point3d(0, 10, 5));

Polyline3d polyline = new Polyline3d(vertices, false); // non fermée

ed.WriteMessage($"\nPolyligne 3D avec {polyline.NumVertices} sommets");
```

### Exemple 2: Polyligne Fermée
```csharp
Point3dCollection vertices = new Point3dCollection();
vertices.Add(new Point3d(0, 0, 0));
vertices.Add(new Point3d(10, 0, 0));
vertices.Add(new Point3d(10, 10, 0));
vertices.Add(new Point3d(0, 10, 0));

Polyline3d closedPolyline = new Polyline3d(vertices, true); // fermée

ed.WriteMessage($"\nPolyligne fermée : {closedPolyline.IsClosed}");
```

## Bonnes Pratiques

1. **Linéaire par Morceaux** : Segments droits, non lisses
2. **Fermée** : Définir à true pour les formes fermées
3. **Sommets** : Minimum 2 sommets requis
4. **Approximation** : Utiliser pour approximer des courbes lisses

## Classes Associées
- **Polyline2d** - Polyligne 2D
- **LineSegment3d** - Segments individuels
- **CubicSplineCurve3d** - Alternative lisse

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Polyline3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Polyline3d)
