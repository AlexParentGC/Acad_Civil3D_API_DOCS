# Structure Extents3d

## Vue d'Ensemble
La structure `Extents3d` représente une boîte englobante 3D définie par des points minimum et maximum. Elle est utilisée pour calculer les limites d'entités, les étendues de zoom et les requêtes spatiales.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `MinPoint` | `Point3d` | Obtient/définit le point de coin minimum |
| `MaxPoint` | `Point3d` | Obtient/définit le point de coin maximum |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `AddPoint(Point3d)` | `void` | Étend les limites pour inclure un point |
| `AddExtents(Extents3d)` | `void` | Étend les limites pour inclure d'autres limites |
| `ExpandBy(Vector3d)` | `void` | Étend les limites par un décalage vectoriel |
| `TransformBy(Matrix3d)` | `void` | Transforme les limites par une matrice |
| `IsEqualTo(Extents3d)` | `bool` | Vérifie l'égalité |

## Exemples de Code

### Exemple 1: Obtenir les Limites d'une Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Obtenir la boîte englobante de l'entité
    Extents3d extents = ent.GeometricExtents;
    
    Point3d min = extents.MinPoint;
    Point3d max = extents.MaxPoint;
    
    // Calculer les dimensions
    double width = max.X - min.X;
    double height = max.Y - min.Y;
    double depth = max.Z - min.Z;
    
    ed.WriteMessage($"\nMin : ({min.X:F2}, {min.Y:F2}, {min.Z:F2})");
    ed.WriteMessage($"\nMax : ({max.X:F2}, {max.Y:F2}, {max.Z:F2})");
    ed.WriteMessage($"\nDimensions : {width:F2} x {height:F2} x {depth:F2}");
    
    tr.Commit();
}
```

### Exemple 2: Combiner Plusieurs Limites d'Entités
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    Extents3d? totalExtents = null;
    
    foreach (ObjectId id in btr)
    {
        Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
        if (ent == null) continue;
        
        try
        {
            Extents3d entExtents = ent.GeometricExtents;
            
            if (totalExtents == null)
                totalExtents = entExtents;
            else
                totalExtents.Value.AddExtents(entExtents);
        }
        catch { }
    }
    
    if (totalExtents != null)
    {
        ed.WriteMessage($"\nLimites totales :");
        ed.WriteMessage($"\n  Min : {totalExtents.Value.MinPoint}");
        ed.WriteMessage($"\n  Max : {totalExtents.Value.MaxPoint}");
    }
    
    tr.Commit();
}
```

### Exemple 3: Étendre les Limites
```csharp
Point3d min = new Point3d(0, 0, 0);
Point3d max = new Point3d(10, 10, 0);
Extents3d extents = new Extents3d(min, max);

// Ajouter un point (étend si en dehors des limites actuelles)
Point3d newPoint = new Point3d(15, 5, 0);
extents.AddPoint(newPoint);

ed.WriteMessage($"\nMax étendu : {extents.MaxPoint}"); // (15, 10, 0)

// Étendre par décalage
Vector3d offset = new Vector3d(1, 1, 1);
extents.ExpandBy(offset);

ed.WriteMessage($"\nAprès extension : Min={extents.MinPoint}, Max={extents.MaxPoint}");
```

### Exemple 4: Zoom sur les Limites
```csharp
[CommandMethod("ZOOMEXTENTS")]
public void ZoomToExtents()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
        
        Extents3d? extents = null;
        
        foreach (ObjectId id in btr)
        {
            Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
            if (ent == null) continue;
            
            try
            {
                if (extents == null)
                    extents = ent.GeometricExtents;
                else
                    extents.Value.AddExtents(ent.GeometricExtents);
            }
            catch { }
        }
        
        if (extents != null)
        {
            // Zoom sur les limites
            Point3d min = extents.Value.MinPoint;
            Point3d max = extents.Value.MaxPoint;
            
            ViewTableRecord view = ed.GetCurrentView();
            view.CenterPoint = new Point2d(
                (min.X + max.X) / 2,
                (min.Y + max.Y) / 2
            );
            
            view.Height = max.Y - min.Y;
            view.Width = max.X - min.X;
            
            ed.SetCurrentView(view);
        }
        
        tr.Commit();
    }
}
```

## Bonnes Pratiques

1. **Vérifications Null** : Vérifier null lors de l'obtention des limites d'entités
2. **Try-Catch** : Certaines entités peuvent ne pas avoir de limites valides
3. **Combinaison** : Utiliser `AddExtents()` pour combiner plusieurs boîtes englobantes
4. **Marges** : Utiliser `ExpandBy()` pour ajouter des marges autour des limites

## Classes Associées
- **Point3d** - Points de coin min/max
- **Vector3d** - Pour étendre les limites
- **Entity** - Toutes les entités ont la propriété `GeometricExtents`
- **Extents2d** - Boîte englobante 2D

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Structure Extents3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Extents3d)
