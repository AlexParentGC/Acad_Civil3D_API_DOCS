# Classe Spline

## Vue d'Ensemble
La classe `Spline` représente une courbe lisse définie par des points de contrôle et des points de lissage dans AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Spline
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Degree` | `int` | Obtient le degré de la spline |
| `Rational` | `bool` | Obtient si la spline est rationnelle (NURBS) |
| `Closed` | `bool` | Obtient/définit si la spline est fermée |
| `Periodic` | `bool` | Obtient si la spline est périodique |
| `NumControlPoints` | `int` | Obtient le nombre de points de contrôle |
| `NumFitPoints` | `int` | Obtient le nombre de points de lissage |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SetControlPointAt(int, Point3d)` | `void` | Définit un point de contrôle |
| `GetControlPointAt(int)` | `Point3d` | Obtient un point de contrôle |
| `SetFitPointAt(int, Point3d)` | `void` | Définit un point de lissage |
| `GetFitPointAt(int)` | `Point3d` | Obtient un point de lissage |

## Exemples de Code

### Exemple 1: Créer une Spline à partir de Points de Lissage
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3dCollection fitPoints = new Point3dCollection();
    fitPoints.Add(new Point3d(0, 0, 0));
    fitPoints.Add(new Point3d(50, 50, 0));
    fitPoints.Add(new Point3d(100, 25, 0));
    fitPoints.Add(new Point3d(150, 75, 0));
    fitPoints.Add(new Point3d(200, 50, 0));
    
    Spline spline = new Spline(fitPoints, 3, 0.0); // Degré 3, tolérance 0
    
    btr.AppendEntity(spline);
    tr.AddNewlyCreatedDBObject(spline, true);
    
    tr.Commit();
}
```

## Objets Associés
- [Polyline](../Polylines/Polyline.md) - Ligne multi-segments
- [Curve](../../BaseClasses/Curve.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Spline)
