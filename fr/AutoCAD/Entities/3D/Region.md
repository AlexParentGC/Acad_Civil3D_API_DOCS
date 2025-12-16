# Classe Region

## Vue d'Ensemble
`Region` représente une zone plane délimitée avec des propriétés physiques (Aire, Périmètre, Centroïde, Inertie). Les régions sont créées à partir de boucles fermées de courbes (lignes, arcs, polylignes) et forment la base pour créer des formes 2D complexes qui peuvent être extrudées en objets `Solid3d`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Region
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Area` | `double` | Aire totale de la région. |
| `Perimeter` | `double` | Longueur totale des contours. |
| `Normal` | `Vector3d` | Vecteur normal du plan de la région. |
| `AreaProperties` | `RegionAreaProperties` | Propriétés physiques complexes. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `CreateFromCurves(DBObjectCollection)` | `DBObjectCollection` | Méthode statique pour créer des régions à partir de courbes. |
| `BooleanOperation(BooleanOperationType, Region)` | `void` | Union, Soustraction, Intersection. |
| `GetAreaProp(Point3d, Vector3d, Vector3d)` | `void` | Calcule les propriétés d'aire relatives aux axes. |

## Exemples de Code

### Exemple 1: Créer une Région à partir d'un Cercle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Définir la courbe d'entrée
    Circle circ = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    DBObjectCollection curves = new DBObjectCollection();
    curves.Add(circ);
    
    // Créer la Région
    DBObjectCollection regions = Region.CreateFromCurves(curves);
    
    if (regions.Count > 0)
    {
        Region reg = regions[0] as Region;
        btr.AppendEntity(reg);
        tr.AddNewlyCreatedDBObject(reg, true);
    }
    
    // Libérer la collection (mais PAS la région que nous avons ajoutée à la BD)
    regions.Dispose();
    tr.Commit();
}
```

### Exemple 2: Intersection Booléenne
```csharp
// Deux régions se chevauchant
Region r1 = ...;
Region r2 = ...;

// Intersecter : Le résultat est la zone commune
r1.BooleanOperation(BooleanOperationType.BoolIntersect, r2);

// r1 est maintenant l'intersection
// r2 est invalidé
r2.Dispose();
```

### Exemple 3: Calculer l'Aire
```csharp
public void ShowArea(Region reg)
{
    ed.WriteMessage($"\nAire : {reg.Area}");
    ed.WriteMessage($"\nPérimètre : {reg.Perimeter}");
}
```

### Exemple 4: Créer une Forme Complexe (Soustraite)
```csharp
// Créer un carré
DBObjectCollection p1 = new DBObjectCollection();
p1.Add(new Polyline(4)); // ... supposons un carré 10x10 ...
Region outer = Region.CreateFromCurves(p1)[0] as Region;

// Créer un cercle
DBObjectCollection p2 = new DBObjectCollection();
p2.Add(new Circle(center, normal, 2.0));
Region inner = Region.CreateFromCurves(p2)[0] as Region;

// Soustraire le trou du carré
outer.BooleanOperation(BooleanOperationType.BoolSubtract, inner);
inner.Dispose();

// Résultat : Carré avec un trou
```

### Exemple 5: Exploser une Région
```csharp
DBObjectCollection parts = new DBObjectCollection();
region.Explode(parts);

// Le résultat est généralement les courbes de contour (Lignes, Arcs, Splines)
foreach (Entity e in parts)
{
    // ...
}
```

### Exemple 6: Vérification de Coplanarité (Prérequis)
```csharp
// Region.CreateFromCurves échoue si les courbes ne sont pas coplanaires
// ou non fermées.
// Validez toujours les courbes d'entrée.
```

### Exemple 7: Extruder en Solide
```csharp
Region reg = ...;
Solid3d sol = new Solid3d();
sol.CreateExtrudedSolid(reg, Vector3d.ZAxis * 5.0, new SweepOptions());
```

### Exemple 8: Analyse des Propriétés de Masse
```csharp
Plane plane = new Plane(Point3d.Origin, Vector3d.ZAxis);
Solid3dMassProperties props = reg.GetMassProperties();

ed.WriteMessage($"\nCentroïde : {props.Centroid}");
ed.WriteMessage($"\nMoments : {props.PrincipalMoments}");
```

## Meilleures Pratiques
1. **Validation d'Entrée** : `CreateFromCurves` est strict. Les courbes doivent former une boucle fermée, être planes, et ne pas s'auto-intersecter.
2. **Libération (Disposal)** : Comme `Solid3d`, les Régions enveloppent des entités ACIS natives. Libérez si non persistant.
3. **Opérations Booléennes** : Les opérandes doivent être sur le même plan.
4. **Collections** : `CreateFromCurves` retourne une collection car un ensemble de courbes peut résulter en plusieurs régions disjointes (ex: deux cercles). Gérez toutes les régions retournées.

## Objets Associés
- [Solid3d](Solid3d.md) - Équivalent 3D.
- [Hatch](../Complex/Hatch.md) - Utilise souvent des contours similaires aux Régions.

## Références
- [Référence Autodesk Region](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Region)
