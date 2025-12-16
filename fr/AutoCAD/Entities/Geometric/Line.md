# Classe Line

## Vue d'Ensemble
La classe `Line` représente un segment de ligne droite dans AutoCAD. C'est l'une des entités géométriques les plus fondamentales, définie par deux points d'extrémité.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Line
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `StartPoint` | `Point3d` | Obtient/définit le point de départ de la ligne |
| `EndPoint` | `Point3d` | Obtient/définit le point de fin de la ligne |
| `Delta` | `Vector3d` | Obtient le vecteur du point de départ au point de fin |
| `Length` | `double` | Obtient la longueur de la ligne |
| `Angle` | `double` | Obtient l'angle de la ligne en radians |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |
| `Thickness` | `double` | Obtient/définit l'épaisseur (pour extrusion 3D) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d, bool)` | `Point3d` | Obtient le point le plus proche sur la ligne par rapport à un point donné |
| `GetDistAtPoint(Point3d)` | `double` | Obtient la distance le long de la ligne jusqu'à un point |
| `GetPointAtDist(double)` | `Point3d` | Obtient un point à une distance spécifiée le long de la ligne |
| `GetPointAtParameter(double)` | `Point3d` | Obtient un point à une valeur de paramètre (0-1) |

## Exemples de Code

### Exemple 1: Créer une Ligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer une ligne de (0,0,0) à (100,100,0)
    Line line = new Line(new Point3d(0, 0, 0), new Point3d(100, 100, 0));
    
    // Définir les propriétés
    line.Layer = "0";
    line.ColorIndex = 1; // Rouge
    
    // Ajouter à la base de données
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    tr.Commit();
}
```

### Exemple 2: Modifier les Points d'Extrémité de la Ligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForWrite) as Line;
    
    // Déplacer le point de départ
    line.StartPoint = new Point3d(10, 10, 0);
    
    // Déplacer le point de fin
    line.EndPoint = new Point3d(200, 150, 0);
    
    tr.Commit();
}
```

### Exemple 3: Obtenir les Propriétés de la Ligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForRead) as Line;
    
    double length = line.Length;
    double angle = line.Angle * (180.0 / Math.PI); // Convertir en degrés
    Vector3d direction = line.Delta.GetNormal(); // Vecteur direction normalisé
    
    Point3d midPoint = line.GetPointAtParameter(0.5); // Point médian
    
    ed.WriteMessage($"\nLongueur Ligne : {length:F2}");
    ed.WriteMessage($"\nAngle Ligne : {angle:F2}°");
    ed.WriteMessage($"\nPoint médian : ({midPoint.X:F2}, {midPoint.Y:F2})");
    
    tr.Commit();
}
```

### Exemple 4: Trouver le Point le Plus Proche sur la Ligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForRead) as Line;
    
    Point3d testPoint = new Point3d(50, 75, 0);
    
    // Obtenir le point le plus proche sur la ligne (extend = false signifie ne pas étendre au-delà des extrémités)
    Point3d closestPoint = line.GetClosestPointTo(testPoint, false);
    
    double distance = testPoint.DistanceTo(closestPoint);
    
    ed.WriteMessage($"\nPoint le plus proche : ({closestPoint.X:F2}, {closestPoint.Y:F2})");
    ed.WriteMessage($"\nDistance : {distance:F2}");
    
    tr.Commit();
}
```

### Exemple 5: Créer Plusieurs Lignes (Rectangle)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d p1 = new Point3d(0, 0, 0);
    Point3d p2 = new Point3d(100, 0, 0);
    Point3d p3 = new Point3d(100, 50, 0);
    Point3d p4 = new Point3d(0, 50, 0);
    
    // Créer quatre lignes formant un rectangle
    Line[] lines = new Line[]
    {
        new Line(p1, p2),
        new Line(p2, p3),
        new Line(p3, p4),
        new Line(p4, p1)
    };
    
    foreach (Line line in lines)
    {
        btr.AppendEntity(line);
        tr.AddNewlyCreatedDBObject(line, true);
    }
    
    tr.Commit();
}
```

## Modèles Courants

### Créer une Ligne à un Angle
```csharp
Point3d startPoint = new Point3d(0, 0, 0);
double length = 100.0;
double angleInDegrees = 45.0;
double angleInRadians = angleInDegrees * (Math.PI / 180.0);

Point3d endPoint = new Point3d(
    startPoint.X + length * Math.Cos(angleInRadians),
    startPoint.Y + length * Math.Sin(angleInRadians),
    startPoint.Z
);

Line line = new Line(startPoint, endPoint);
```

### Étendre une Ligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForWrite) as Line;
    
    // Étendre de 10 unités dans la direction de la ligne
    Vector3d direction = line.Delta.GetNormal();
    line.EndPoint = line.EndPoint + (direction * 10.0);
    
    tr.Commit();
}
```

## Objets Associés
- [Curve](../../BaseClasses/Curve.md) - Classe de base pour Line
- [Entity](../../BaseClasses/Entity.md) - Classe de base pour toutes les entités
- [Polyline](../Polylines/Polyline.md) - Plusieurs segments de ligne connectés
- [Arc](Arc.md) - Contrepartie courbe de Line

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Line)
