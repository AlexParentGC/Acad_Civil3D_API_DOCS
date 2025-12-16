# Classe Circle

## Vue d'Ensemble
La classe `Circle` représente une entité circulaire dans AutoCAD, définie par un point central et un rayon.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Circle
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Center` | `Point3d` | Obtient/définit le point central du cercle |
| `Radius` | `double` | Obtient/définit le rayon |
| `Diameter` | `double` | Obtient/définit le diamètre |
| `Circumference` | `double` | Obtient la circonférence (lecture seule) |
| `Area` | `double` | Obtient l'aire (lecture seule) |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal (définit le plan) |
| `Thickness` | `double` | Obtient/définit l'épaisseur (pour extrusion 3D) |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetClosestPointTo(Point3d, bool)` | `Point3d` | Obtient le point le plus proche sur le cercle |
| `GetPointAtParameter(double)` | `Point3d` | Obtient un point au paramètre (0-2π) |
| `GetDistAtPoint(Point3d)` | `double` | Obtient la distance le long du cercle jusqu'à un point |

## Exemples de Code

### Exemple 1: Créer un Cercle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer un cercle à (50, 50, 0) avec rayon 25
    Circle circle = new Circle();
    circle.Center = new Point3d(50, 50, 0);
    circle.Radius = 25.0;
    
    // Définir les propriétés
    circle.Layer = "0";
    circle.ColorIndex = 2; // Jaune
    
    // Ajouter à la base de données
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}
```

### Exemple 2: Obtenir les Propriétés d'un Cercle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    Point3d center = circle.Center;
    double radius = circle.Radius;
    double diameter = circle.Diameter;
    double circumference = circle.Circumference;
    double area = circle.Area;
    
    ed.WriteMessage($"\nCentre : ({center.X:F2}, {center.Y:F2})");
    ed.WriteMessage($"\nRayon : {radius:F2}");
    ed.WriteMessage($"\nDiamètre : {diameter:F2}");
    ed.WriteMessage($"\nCirconférence : {circumference:F2}");
    ed.WriteMessage($"\nAire : {area:F2}");
    
    tr.Commit();
}
```

### Exemple 3: Obtenir des Points sur le Cercle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    // Obtenir les points à 0°, 90°, 180°, 270°
    Point3d pt0 = circle.GetPointAtParameter(0); // 0 degrés
    Point3d pt90 = circle.GetPointAtParameter(Math.PI / 2); // 90 degrés
    Point3d pt180 = circle.GetPointAtParameter(Math.PI); // 180 degrés
    Point3d pt270 = circle.GetPointAtParameter(3 * Math.PI / 2); // 270 degrés
    
    tr.Commit();
}
```

### Exemple 4: Créer des Cercles Concentriques
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d center = new Point3d(100, 100, 0);
    
    // Créer 5 cercles concentriques
    for (int i = 1; i <= 5; i++)
    {
        Circle circle = new Circle();
        circle.Center = center;
        circle.Radius = i * 10.0;
        
        btr.AppendEntity(circle);
        tr.AddNewlyCreatedDBObject(circle, true);
    }
    
    tr.Commit();
}
```

### Exemple 5: Modifier un Cercle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForWrite) as Circle;
    
    // Déplacer le centre
    circle.Center = new Point3d(75, 75, 0);
    
    // Changer le rayon
    circle.Radius = 50.0;
    
    // Ou définir le diamètre à la place
    circle.Diameter = 100.0; // Même que rayon 50
    
    tr.Commit();
}
```

## Modèles Courants

### Créer un Cercle à partir de 3 Points
```csharp
// Calculer le cercle à partir de 3 points (nécessite calculs géométriques)
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(100, 0, 0);
Point3d p3 = new Point3d(50, 50, 0);

CircularArc3d arc3d = new CircularArc3d(p1, p2, p3);
Point3d center = arc3d.Center;
double radius = arc3d.Radius;

Circle circle = new Circle();
circle.Center = center;
circle.Radius = radius;
```

### Vérifier si un Point est dans le Cercle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    Point3d testPoint = new Point3d(60, 60, 0);
    double distance = circle.Center.DistanceTo(testPoint);
    
    bool isInside = distance <= circle.Radius;
    
    ed.WriteMessage($"\nLe point est {(isInside ? "dedans" : "dehors")} le cercle");
    
    tr.Commit();
}
```

## Objets Associés
- [Arc](Arc.md) - Cercle partiel
- [Ellipse](Ellipse.md) - Forme ovale
- [Curve](Curve.md) - Classe de base
- [Entity](Entity.md) - Classe de base pour toutes les entités

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Circle)
