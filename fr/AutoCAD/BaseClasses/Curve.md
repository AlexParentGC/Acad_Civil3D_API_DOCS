# Classe Curve

## Vue d'Ensemble
La classe `Curve` est la classe de base pour toutes les entités géométriques courbes dans AutoCAD, incluant les lignes, arcs, cercles, polylignes, ellipses et splines. Elle fournit des propriétés et méthodes communes pour les courbes.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  ├─ Line
                  ├─ Arc
                  ├─ Circle
                  ├─ Ellipse
                  ├─ Spline
                  ├─ Polyline
                  ├─ Polyline2d
                  └─ Polyline3d
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `StartPoint` | `Point3d` | Obtient le point de départ de la courbe |
| `EndPoint` | `Point3d` | Obtient le point de fin de la courbe |
| `StartParam` | `double` | Obtient la valeur du paramètre de départ |
| `EndParam` | `double` | Obtient la valeur du paramètre de fin |
| `Closed` | `bool` | Indique si la courbe est fermée |
| `Periodic` | `bool` | Indique si la courbe est périodique |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetPointAtParameter(double)` | `Point3d` | Obtient le point à la valeur du paramètre |
| `GetParameterAtPoint(Point3d)` | `double` | Obtient le paramètre au point |
| `GetDistAtParam(double)` | `double` | Obtient la distance le long de la courbe au paramètre |
| `GetParamAtDist(double)` | `double` | Obtient le paramètre à la distance |
| `GetPointAtDist(double)` | `Point3d` | Obtient le point à la distance le long de la courbe |
| `GetDistAtPoint(Point3d)` | `double` | Obtient la distance le long de la courbe jusqu'au point |
| `GetClosestPointTo(Point3d, bool)` | `Point3d` | Obtient le point le plus proche sur la courbe |
| `GetFirstDerivative(double)` | `Vector3d` | Obtient le vecteur tangent au paramètre |
| `GetSecondDerivative(double)` | `Vector3d` | Obtient le vecteur de courbure au paramètre |
| `GetSplitCurves(Point3dCollection)` | `DBObjectCollection` | Divise la courbe aux points |
| `Extend(double)` | `void` | Prolonge la courbe jusqu'au paramètre |
| `Extend(bool, Point3d)` | `void` | Prolonge la courbe jusqu'au point |

## Exemples de Code

### Exemple 1: Obtenir des Points le Long d'une Courbe
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    
    // Obtenir des points à intervalles réguliers
    double totalLength = curve.GetDistAtParam(curve.EndParam);
    int numPoints = 10;
    
    for (int i = 0; i <= numPoints; i++)
    {
        double distance = (totalLength / numPoints) * i;
        Point3d point = curve.GetPointAtDist(distance);
        
        ed.WriteMessage($"\nPoint {i} : ({point.X:F2}, {point.Y:F2})");
    }
    
    tr.Commit();
}
```

### Exemple 2: Trouver le Point le Plus Proche sur la Courbe
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    
    Point3d testPoint = new Point3d(100, 100, 0);
    
    // Obtenir le point le plus proche (extend = false signifie ne pas prolonger au-delà des extrémités)
    Point3d closestPoint = curve.GetClosestPointTo(testPoint, false);
    
    double distance = testPoint.DistanceTo(closestPoint);
    
    ed.WriteMessage($"\nPoint le plus proche : ({closestPoint.X:F2}, {closestPoint.Y:F2})");
    ed.WriteMessage($"\nDistance : {distance:F2}");
    
    tr.Commit();
}
```

### Exemple 3: Obtenir la Direction Tangente
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    
    // Obtenir la tangente au point médian
    double midParam = (curve.StartParam + curve.EndParam) / 2;
    Vector3d tangent = curve.GetFirstDerivative(midParam);
    
    // Normaliser en vecteur unitaire
    tangent = tangent.GetNormal();
    
    double angle = Math.Atan2(tangent.Y, tangent.X) * (180.0 / Math.PI);
    
    ed.WriteMessage($"\nAngle tangent au point médian : {angle:F2}°");
    
    tr.Commit();
}
```

### Exemple 4: Diviser une Courbe
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer les points de division
    Point3dCollection splitPoints = new Point3dCollection();
    splitPoints.Add(curve.GetPointAtDist(curve.GetDistAtParam(curve.EndParam) * 0.33));
    splitPoints.Add(curve.GetPointAtDist(curve.GetDistAtParam(curve.EndParam) * 0.66));
    
    // Diviser la courbe
    DBObjectCollection splitCurves = curve.GetSplitCurves(splitPoints);
    
    // Ajouter les courbes divisées à la base de données
    foreach (DBObject obj in splitCurves)
    {
        Curve splitCurve = obj as Curve;
        btr.AppendEntity(splitCurve);
        tr.AddNewlyCreatedDBObject(splitCurve, true);
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Entity](Entity.md) - Classe de base
- [Line](../Entities/Geometric/Line.md) - Classe dérivée
- [Arc](../Entities/Geometric/Arc.md) - Classe dérivée
- [Circle](../Entities/Geometric/Circle.md) - Classe dérivée
- [Polyline](../Entities/Polylines/Polyline.md) - Classe dérivée

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Curve)
