# Classe Polyline

## Vue d'Ensemble
La classe `Polyline` représente une polyligne 2D légère dans AutoCAD. C'est une version optimisée qui utilise moins de mémoire que l'ancienne classe Polyline2d.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Polyline
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `NumberOfVertices` | `int` | Obtient le nombre de sommets |
| `Closed` | `bool` | Obtient/définit si la polyligne est fermée |
| `Length` | `double` | Obtient la longueur totale |
| `Area` | `double` | Obtient l'aire (si fermée) |
| `Elevation` | `double` | Obtient/définit l'élévation (valeur Z) |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |
| `Thickness` | `double` | Obtient/définit l'épaisseur |
| `ConstantWidth` | `double` | Obtient/définit la largeur constante pour tous les segments |
| `HasWidth` | `bool` | Obtient si la polyligne a une largeur |
| `HasBulges` | `bool` | Obtient si la polyligne a des courbures (arcs) |
| `Plinegen` | `bool` | Obtient/définit le mode de génération de type de ligne |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `AddVertexAt(int, Point2d, double, double, double)` | `void` | Ajoute un sommet à l'index spécifié |
| `RemoveVertexAt(int)` | `void` | Supprime le sommet à l'index |
| `GetPoint2dAt(int)` | `Point2d` | Obtient le point 2D à l'index du sommet |
| `GetPoint3dAt(int)` | `Point3d` | Obtient le point 3D à l'index du sommet |
| `SetPointAt(int, Point2d)` | `void` | Définit le point à l'index du sommet |
| `GetBulgeAt(int)` | `double` | Obtient la valeur de courbure au sommet |
| `SetBulgeAt(int, double)` | `void` | Définit la valeur de courbure au sommet |
| `GetStartWidthAt(int)` | `double` | Obtient la largeur de départ au segment |
| `GetEndWidthAt(int)` | `double` | Obtient la largeur de fin au segment |
| `SetWidthsAt(int, double, double)` | `void` | Définit les largeurs de départ/fin au segment |
| `GetSegmentType(int)` | `SegmentType` | Obtient le type de segment (Ligne ou Arc) |

## Exemples de Code

### Exemple 1: Créer une Polyligne Simple
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline pline = new Polyline();
    
    // Ajouter des sommets
    pline.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0);
    pline.AddVertexAt(1, new Point2d(100, 0), 0, 0, 0);
    pline.AddVertexAt(2, new Point2d(100, 50), 0, 0, 0);
    pline.AddVertexAt(3, new Point2d(0, 50), 0, 0, 0);
    
    // Fermer la polyligne
    pline.Closed = true;
    
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}
```

### Exemple 2: Créer une Polyligne avec Courbures (Arcs)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline pline = new Polyline();
    
    // Ajouter des sommets avec courbures
    // Courbure (Bulge) = tan(angle/4), où angle est l'angle inclus
    // Courbure de 1 = demi-cercle de 180°
    pline.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0); // Segment droit
    pline.AddVertexAt(1, new Point2d(100, 0), 1, 0, 0); // Segment arc (courbure=1)
    pline.AddVertexAt(2, new Point2d(100, 100), 0, 0, 0); // Segment droit
    
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}
```

### Exemple 3: Créer une Polyligne avec Largeur
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline pline = new Polyline();
    
    // Ajouter des sommets avec largeurs variables
    pline.AddVertexAt(0, new Point2d(0, 0), 0, 5, 5); // Largeur 5
    pline.AddVertexAt(1, new Point2d(100, 0), 0, 5, 10); // Effilage de 5 à 10
    pline.AddVertexAt(2, new Point2d(100, 100), 0, 10, 10); // Largeur 10
    
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}
```

### Exemple 4: Itérer à Travers les Sommets de Polyligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = tr.GetObject(plineId, OpenMode.ForRead) as Polyline;
    
    int numVertices = pline.NumberOfVertices;
    
    for (int i = 0; i < numVertices; i++)
    {
        Point2d pt2d = pline.GetPoint2dAt(i);
        Point3d pt3d = pline.GetPoint3dAt(i);
        double bulge = pline.GetBulgeAt(i);
        SegmentType segType = pline.GetSegmentType(i);
        
        ed.WriteMessage($"\nSommet {i} : ({pt2d.X:F2}, {pt2d.Y:F2})");
        ed.WriteMessage($"\n  Courbure : {bulge:F4}");
        ed.WriteMessage($"\n  Type Segment : {segType}");
    }
    
    tr.Commit();
}
```

### Exemple 5: Modifier les Sommets de Polyligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = tr.GetObject(plineId, OpenMode.ForWrite) as Polyline;
    
    // Déplacer un sommet
    pline.SetPointAt(1, new Point2d(150, 50));
    
    // Ajouter un nouveau sommet
    pline.AddVertexAt(2, new Point2d(125, 75), 0, 0, 0);
    
    // Supprimer un sommet
    pline.RemoveVertexAt(0);
    
    // Définir la courbure pour le segment d'arc
    pline.SetBulgeAt(1, 0.5);
    
    tr.Commit();
}
```

### Exemple 6: Obtenir l'Aire et la Longueur de la Polyligne
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = tr.GetObject(plineId, OpenMode.ForRead) as Polyline;
    
    double length = pline.Length;
    double area = pline.Closed ? pline.Area : 0;
    
    ed.WriteMessage($"\nLongueur Polyligne : {length:F2}");
    if (pline.Closed)
    {
        ed.WriteMessage($"\nAire Polyligne : {area:F2}");
    }
    
    tr.Commit();
}
```

## Modèles Courants

### Créer un Rectangle
```csharp
Polyline rect = new Polyline();
rect.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0);
rect.AddVertexAt(1, new Point2d(100, 0), 0, 0, 0);
rect.AddVertexAt(2, new Point2d(100, 50), 0, 0, 0);
rect.AddVertexAt(3, new Point2d(0, 50), 0, 0, 0);
rect.Closed = true;
```

### Convertir des Segments de Ligne en Polyligne
```csharp
// En supposant que vous avez des segments de ligne connectés
Polyline pline = new Polyline();
int index = 0;
foreach (Line line in lines)
{
    Point2d pt = new Point2d(line.StartPoint.X, line.StartPoint.Y);
    pline.AddVertexAt(index++, pt, 0, 0, 0);
}
// Ajouter le dernier point d'extrémité
Point2d lastPt = new Point2d(lines.Last().EndPoint.X, lines.Last().EndPoint.Y);
pline.AddVertexAt(index, lastPt, 0, 0, 0);
```

## Objets Associés
- [Polyline2d](Polyline2d.md) - Format de polyligne 2D plus ancien
- [Polyline3d](Polyline3d.md) - Polyligne 3D
- [Line](Line.md) - Segment de ligne unique
- [Curve](Curve.md) - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Polyline)
