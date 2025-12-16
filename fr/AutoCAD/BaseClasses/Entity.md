# Classe Entity

## Vue d'Ensemble
La classe `Entity` est la classe de base pour tous les objets graphiques dans AutoCAD. Elle hérite de `DBObject` et fournit des propriétés et méthodes communes pour toutes les entités dessinables comme les lignes, cercles, textes, etc.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Curve (Ligne, Arc, Cercle, Polyligne, etc.)
              ├─ BlockReference
              ├─ DBText
              ├─ MText
              ├─ Dimension
              ├─ Hatch
              └─ ... (toutes les entités graphiques)
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Layer` | `string` | Obtient/définit le nom du calque |
| `LayerId` | `ObjectId` | Obtient/définit l'ObjectId du calque |
| `Color` | `Color` | Obtient/définit la couleur de l'entité |
| `ColorIndex` | `short` | Obtient/définit l'index de couleur (0-256) |
| `Linetype` | `string` | Obtient/définit le nom du type de ligne |
| `LinetypeId` | `ObjectId` | Obtient/définit l'ObjectId du type de ligne |
| `LinetypeScale` | `double` | Obtient/définit l'échelle du type de ligne |
| `Lineweight` | `LineWeight` | Obtient/définit l'épaisseur de ligne |
| `Visible` | `bool` | Obtient/définit la visibilité |
| `Transparency` | `Transparency` | Obtient/définit la transparence |
| `Material` | `string` | Obtient/définit le nom du matériau |
| `MaterialId` | `ObjectId` | Obtient/définit l'ObjectId du matériau |
| `PlotStyleName` | `string` | Obtient/définit le nom du style de tracé |
| `Bounds` | `Extents3d?` | Obtient les limites de la boîte englobante |
| `GeometricExtents` | `Extents3d` | Obtient les étendues géométriques |
| `BlockId` | `ObjectId` | Obtient l'ObjectId du BlockTableRecord propriétaire |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetBoundingBox()` | `void` | Obtient la boîte englobante (paramètres out) |
| `GetGeometricExtents()` | `void` | Obtient les étendues géométriques (paramètres out) |
| `Highlight()` | `void` | Met en surbrillance l'entité |
| `Unhighlight()` | `void` | Supprime la surbrillance |
| `GetGripPoints()` | `void` | Obtient les points de poignée pour l'entité |
| `MoveGripPointsAt()` | `void` | Déplace les points de poignée |
| `Intersect()` | `void` | Trouve les points d'intersection avec une autre entité |
| `GetTransformedCopy()` | `Entity` | Obtient une copie transformée de l'entité |
| `TransformBy()` | `void` | Transforme l'entité par une matrice |
| `Draw()` | `void` | Dessine l'entité |

## Exemples de Code

### Exemple 1: Changer les Propriétés d'une Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Changer calque
    ent.Layer = "NouveauCalque";
    
    // Changer couleur en rouge
    ent.ColorIndex = 1;
    
    // Changer type de ligne
    ent.Linetype = "DASHED";
    
    // Changer échelle type de ligne
    ent.LinetypeScale = 2.0;
    
    // Changer épaisseur ligne
    ent.Lineweight = LineWeight.LineWeight050;
    
    // Définir transparence (0-255, où 0 est opaque)
    ent.Transparency = new Transparency(128);
    
    tr.Commit();
}
```

### Exemple 2: Obtenir les Limites de l'Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Obtenir étendues géométriques
    Extents3d extents = ent.GeometricExtents;
    
    Point3d minPoint = extents.MinPoint;
    Point3d maxPoint = extents.MaxPoint;
    
    double width = maxPoint.X - minPoint.X;
    double height = maxPoint.Y - minPoint.Y;
    
    ed.WriteMessage($"\nLimites entité : {width} x {height}");
    
    tr.Commit();
}
```

### Exemple 3: Mettre en Surbrillance des Entités
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Mettre en surbrillance l'entité
    ent.Highlight();
    
    // Attendre entrée utilisateur ou délai
    System.Threading.Thread.Sleep(2000);
    
    // Supprimer surbrillance
    ent.Unhighlight();
    
    tr.Commit();
}
```

### Exemple 4: Transformer des Entités
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Créer une matrice de transformation (déplacer 10 unités en X, 5 en Y)
    Matrix3d transform = Matrix3d.Displacement(new Vector3d(10, 5, 0));
    
    // Appliquer transformation
    ent.TransformBy(transform);
    
    tr.Commit();
}
```

### Exemple 5: Trouver des Intersections
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent1 = tr.GetObject(entityId1, OpenMode.ForRead) as Entity;
    Entity ent2 = tr.GetObject(entityId2, OpenMode.ForRead) as Entity;
    
    Point3dCollection intersectionPoints = new Point3dCollection();
    
    // Trouver intersections (Mode Extend : None, Both, First, Second)
    ent1.IntersectWith(ent2, Intersect.OnBothOperands, intersectionPoints, IntPtr.Zero, IntPtr.Zero);
    
    foreach (Point3d pt in intersectionPoints)
    {
        ed.WriteMessage($"\nIntersection à : {pt}");
    }
    
    tr.Commit();
}
```

### Exemple 6: Itérer à Travers Toutes les Entités
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    foreach (ObjectId objId in modelSpace)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        ed.WriteMessage($"\nType Entité : {ent.GetType().Name}");
        ed.WriteMessage($"\n  Calque : {ent.Layer}");
        ed.WriteMessage($"\n  Index Couleur : {ent.ColorIndex}");
    }
    
    tr.Commit();
}
```

### Exemple 7: Filtrer les Entités par Type
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    // Compter entités par type
    int lineCount = 0;
    int circleCount = 0;
    int textCount = 0;
    
    foreach (ObjectId objId in btr)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        if (ent is Line) lineCount++;
        else if (ent is Circle) circleCount++;
        else if (ent is DBText || ent is MText) textCount++;
    }
    
    ed.WriteMessage($"\nLignes : {lineCount}, Cercles : {circleCount}, Texte : {textCount}");
    
    tr.Commit();
}
```

## Modèles Courants

### Assignation de Couleur
```csharp
// Par index de couleur (ACI - AutoCAD Color Index)
ent.ColorIndex = 1; // Rouge
ent.ColorIndex = 256; // DuCalque
ent.ColorIndex = 0; // DuBloc

// Par vraie couleur
ent.Color = Color.FromRgb(255, 0, 0); // Rouge

// Par carnet de couleurs
ent.Color = Color.FromColorIndex(ColorMethod.ByAci, 1);
```

### Assignation de Calque
```csharp
// Par nom de calque
ent.Layer = "0"; // Calque 0

// Par ObjectId de calque
LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
if (lt.Has("MonCalque"))
{
    ent.LayerId = lt["MonCalque"];
}
```

## Objets Associés
- [DBObject](DBObject.md) - Classe de base pour Entity
- [Curve](Curve.md) - Classe de base pour les entités courbes
- [BlockTableRecord](../SymbolTables/BlockTable.md) - Conteneur pour entités

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Entity)
