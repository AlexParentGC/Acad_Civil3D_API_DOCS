# Classe Point3dCollection

## Vue d'Ensemble
`Point3dCollection` est une collection spécialisée pour les structures `Point3d`. Elle est optimisée pour les opérations géométriques et est requise par de nombreuses méthodes API impliquant des courbes, des solides 3D, et des maillages polygonaux (ex : `GetSplitCurves`, `CreateFromCurves`).

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Count` | `int` | Nombre de points. |
| `this[int]` | `Point3d` | Indexeur pour accéder aux points. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Add(Point3d)` | `int` | Ajoute un point. |
| `Insert(int, Point3d)` | `void` | Insère un point à l'index. |
| `Remove(Point3d)` | `void` | Supprime un point spécifique. |
| `RemoveAt(int)` | `void` | Supprime le point à l'index. |
| `Clear()` | `void` | Vide la liste. |
| `ToArray()` | `Point3d[]` | Exporte un tableau simple. |

## Exemples de Code

### Exemple 1: Usage Basique
```csharp
Point3dCollection pts = new Point3dCollection();
pts.Add(new Point3d(0, 0, 0));
pts.Add(new Point3d(10, 0, 0));
pts.Add(new Point3d(10, 10, 0));

ed.WriteMessage($"\nPolygone défini avec {pts.Count} sommets.");
```

### Exemple 2: Créer une Polyligne 3D
```csharp
Point3dCollection vertices = new Point3dCollection();
// ... remplir sommets ...

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = new Polyline3d(Poly3dType.SimplePoly, vertices, false);
    // ... ajouter à la base de données ...
}
```

### Exemple 3: Définir une Sélection Fence
```csharp
// Les sélections fence nécessitent souvent Point3dCollection
Point3dCollection fence = new Point3dCollection();
fence.Add(p1);
fence.Add(p2);
fence.Add(p3);

PromptSelectionResult res = ed.SelectFence(fence);
```

### Exemple 4: Intersecter des Entités
```csharp
Entity ent1 = ...;
Entity ent2 = ...;

Point3dCollection intersectPts = new Point3dCollection();

// IntersectWith peuple la collection
ent1.IntersectWith(ent2, Intersect.OnBothOperands, intersectPts, IntPtr.Zero, IntPtr.Zero);

foreach (Point3d pt in intersectPts)
{
    ed.WriteMessage($"\nIntersection à : {pt}");
}
```

## Meilleures Pratiques
1. **Libérer** : Implémente `IDisposable`. Utilisez toujours `using` ou appelez `Dispose()` pour libérer les ressources non gérées associées aux géométries sous-jacentes.
2. **Capacité** : Contrairement aux Listes génériques, n'expose pas le contrôle de Capacité facilement, mais est généralement efficace.
3. **Conversion Tableau** : Utilisez `ToArray()` si vous devez passer des données à des méthodes .NET non-AutoCAD.

## Objets Associés
- [Point3d](../Geometry/Point3d.md) - Le type d'élément.
- [ObjectIdCollection](ObjectIdCollection.md) - Collection d'IDs.

## Références
- [Référence Point3dCollection Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Point3dCollection)
