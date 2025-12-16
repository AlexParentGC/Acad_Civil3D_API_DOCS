# Classe Ellipse

## Vue d'Ensemble
La classe `Ellipse` représente une forme elliptique dans AutoCAD, définie par un point central, et des axes majeur et mineur.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Ellipse
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient/définit le point central |
| `MajorAxis` | `Vector3d` | Obtient/définit le vecteur de l'axe majeur |
| `MinorAxis` | `Vector3d` | Obtient le vecteur de l'axe mineur (lecture seule) |
| `MajorRadius` | `double` | Obtient/définit le rayon majeur |
| `MinorRadius` | `double` | Obtient/définit le rayon mineur |
| `RadiusRatio` | `double` | Obtient/définit le ratio rayon mineur sur majeur |
| `StartAngle` | `double` | Obtient/définit l'angle de départ (pour arcs elliptiques) |
| `EndAngle` | `double` | Obtient/définit l'angle de fin (pour arcs elliptiques) |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |

## Exemples de Code

### Exemple 1: Créer une Ellipse
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d center = new Point3d(100, 100, 0);
    Vector3d majorAxis = new Vector3d(50, 0, 0); // 50 unités en direction X
    Vector3d normal = Vector3d.ZAxis;
    double radiusRatio = 0.5; // Rayon mineur est moitié du majeur
    
    Ellipse ellipse = new Ellipse(center, normal, majorAxis, radiusRatio, 0, 2 * Math.PI);
    
    btr.AppendEntity(ellipse);
    tr.AddNewlyCreatedDBObject(ellipse, true);
    
    tr.Commit();
}
```

### Exemple 2: Créer un Arc Elliptique
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d center = new Point3d(200, 200, 0);
    Vector3d majorAxis = new Vector3d(40, 0, 0);
    Vector3d normal = Vector3d.ZAxis;
    double radiusRatio = 0.6;
    double startAngle = 0;
    double endAngle = Math.PI; // Demi ellipse
    
    Ellipse ellipseArc = new Ellipse(center, normal, majorAxis, radiusRatio, startAngle, endAngle);
    
    btr.AppendEntity(ellipseArc);
    tr.AddNewlyCreatedDBObject(ellipseArc, true);
    
    tr.Commit();
}
```

## Objets Associés
- [Circle](Circle.md) - Cas spécial où radiusRatio = 1
- [Arc](Arc.md) - Arc circulaire
- [Curve](../../BaseClasses/Curve.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Ellipse)
