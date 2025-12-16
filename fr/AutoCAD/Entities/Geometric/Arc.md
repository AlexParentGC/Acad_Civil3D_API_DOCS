# Classe Arc

## Vue d'Ensemble
La classe `Arc` représente un arc circulaire dans AutoCAD, défini par un point central, un rayon, un angle de départ et un angle de fin.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Arc
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient/définit le point central |
| `Radius` | `double` | Obtient/définit le rayon |
| `StartAngle` | `double` | Obtient/définit l'angle de départ (radians) |
| `EndAngle` | `double` | Obtient/définit l'angle de fin (radians) |
| `TotalAngle` | `double` | Obtient l'angle total balayé (radians) |
| `StartPoint` | `Point3d` | Obtient le point de départ |
| `EndPoint` | `Point3d` | Obtient le point de fin |
| `Length` | `double` | Obtient la longueur de l'arc |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |
| `Thickness` | `double` | Obtient/définit l'épaisseur |

## Exemples de Code

### Exemple 1: Créer un Arc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer un arc : centre (50,50), rayon 25, de 0° à 90°
    Arc arc = new Arc();
    arc.Center = new Point3d(50, 50, 0);
    arc.Radius = 25.0;
    arc.StartAngle = 0; // 0 degrés
    arc.EndAngle = Math.PI / 2; // 90 degrés
    
    btr.AppendEntity(arc);
    tr.AddNewlyCreatedDBObject(arc, true);
    
    tr.Commit();
}
```

### Exemple 2: Créer un Arc à partir de 3 Points
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d startPt = new Point3d(0, 0, 0);
    Point3d midPt = new Point3d(50, 50, 0);
    Point3d endPt = new Point3d(100, 0, 0);
    
    // Utiliser CircularArc3d pour calculer les paramètres de l'arc
    CircularArc3d arc3d = new CircularArc3d(startPt, midPt, endPt);
    
    Arc arc = new Arc();
    arc.Center = arc3d.Center;
    arc.Radius = arc3d.Radius;
    arc.StartAngle = arc3d.ReferenceVector.AngleOnPlane(new Plane(arc3d.Center, arc3d.Normal));
    arc.EndAngle = arc.StartAngle + (arc3d.EndAngle - arc3d.StartAngle);
    arc.Normal = arc3d.Normal;
    
    btr.AppendEntity(arc);
    tr.AddNewlyCreatedDBObject(arc, true);
    
    tr.Commit();
}
```

### Exemple 3: Obtenir les Propriétés d'un Arc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Arc arc = tr.GetObject(arcId, OpenMode.ForRead) as Arc;
    
    double startAngleDeg = arc.StartAngle * (180.0 / Math.PI);
    double endAngleDeg = arc.EndAngle * (180.0 / Math.PI);
    double totalAngleDeg = arc.TotalAngle * (180.0 / Math.PI);
    double arcLength = arc.Length;
    
    ed.WriteMessage($"\nAngle Départ : {startAngleDeg:F2}°");
    ed.WriteMessage($"\nAngle Fin : {endAngleDeg:F2}°");
    ed.WriteMessage($"\nAngle Total : {totalAngleDeg:F2}°");
    ed.WriteMessage($"\nLongueur Arc : {arcLength:F2}");
    
    tr.Commit();
}
```

## Objets Associés
- [Circle](Circle.md) - Cercle entier
- [Ellipse](Ellipse.md) - Arc elliptique
- [Curve](Curve.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Arc)
