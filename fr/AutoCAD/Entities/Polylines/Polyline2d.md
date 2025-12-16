# Classe Polyline2d

## Vue d'Ensemble
La classe `Polyline2d` représente une entité polyligne 2D héritée dans AutoCAD. Contrairement à la classe légère `Polyline`, `Polyline2d` est un objet conteneur qui possède une collection d'objets `Vertex2d`. Chaque sommet est un objet de base de données séparé. Cette classe est principalement utilisée pour la compatibilité avec les anciens dessins et pour les polylignes nécessitant des données étendues par sommet.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Polyline2d
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `PolyType` | `Poly2dType` | Obtient/définit le type de polyligne (SimplePoly, FitCurvePoly, QuadSplinePoly, CubicSplinePoly) |
| `DefaultStartWidth` | `double` | Obtient/définit la largeur de départ par défaut pour les sommets |
| `DefaultEndWidth` | `double` | Obtient/définit la largeur de fin par défaut pour les sommets |
| `Thickness` | `double` | Obtient/définit l'épaisseur de la polyligne (extrusion) |
| `Elevation` | `double` | Obtient/définit l'élévation (coordonnée Z) |
| `Normal` | `Vector3d` | Obtient/définit le vecteur normal |
| `Closed` | `bool` | Obtient/définit si la polyligne est fermée |
| `LinetypeGenerationOn` | `bool` | Obtient/définit si la génération de type de ligne est continue |
| `Length` | `double` | Obtient la longueur totale de la polyligne |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetEnumerator()` | `IEnumerator` | Obtient un énumérateur pour les sommets |
| `AppendVertex(Vertex2d)` | `void` | Ajoute un sommet à la polyligne |
| `InsertVertexAt(int, Vertex2d)` | `void` | Insère un sommet à l'index spécifié |
| `OpenVertex(Vertex2d, OpenMode)` | `Vertex2d` | Ouvre un sommet en lecture ou écriture |
| `ConvertToPolylineType(Poly2dType)` | `void` | Convertit la polyligne en un type différent |
| `Straighten()` | `void` | Supprime tout lissage de courbe |
| `SplineFit()` | `void` | Applique un lissage spline |

## Exemples de Code

### Exemple 1: Créer une Polyline2d
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer le conteneur de polyligne
    Polyline2d poly = new Polyline2d();
    poly.SetDatabaseDefaults();
    
    // Ajouter la polyligne au dessin
    ObjectId polyId = btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Créer les sommets
    Point3d[] points = new Point3d[]
    {
        new Point3d(0, 0, 0),
        new Point3d(10, 0, 0),
        new Point3d(10, 10, 0),
        new Point3d(0, 10, 0)
    };
    
    foreach (Point3d pt in points)
    {
        Vertex2d vertex = new Vertex2d(pt, 0, 0, 0, 0);
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    // Fermer la polyligne
    poly.Closed = true;
    
    tr.Commit();
}
```

### Exemple 2: Lire les Sommets de Polyline2d
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline2d;
    
    ed.WriteMessage($"\n=== Information Polyline2d ===");
    ed.WriteMessage($"\nType : {poly.PolyType}");
    ed.WriteMessage($"\nFermée : {poly.Closed}");
    ed.WriteMessage($"\nLongueur : {poly.Length:F3}");
    ed.WriteMessage($"\nÉlévation : {poly.Elevation:F3}");
    
    ed.WriteMessage("\n\nSommets :");
    int index = 0;
    
    foreach (ObjectId vertexId in poly)
    {
        Vertex2d vertex = tr.GetObject(vertexId, OpenMode.ForRead) as Vertex2d;
        
        ed.WriteMessage($"\n  Sommet {index} : ({vertex.Position.X:F2}, {vertex.Position.Y:F2})");
        ed.WriteMessage($"\n    Largeur Début : {vertex.StartWidth:F2}");
        ed.WriteMessage($"\n    Largeur Fin : {vertex.EndWidth:F2}");
        ed.WriteMessage($"\n    Courbure : {vertex.Bulge:F3}");
        
        index++;
    }
    
    tr.Commit();
}
```

## Types de Polyline2d

| Type | Description |
|------|-------------|
| `SimplePoly` | Polyligne standard avec segments droits et arcs |
| `FitCurvePoly` | Polyligne lissée passant par les sommets |
| `QuadSplinePoly` | B-spline quadratique |
| `CubicSplinePoly` | B-spline cubique |

## Propriétés Vertex2d

| Propriété | Type | Description |
|-----------|------|-------------|
| `Position` | `Point3d` | Emplacement du sommet |
| `StartWidth` | `double` | Largeur de départ à ce sommet |
| `EndWidth` | `double` | Largeur de fin à ce sommet |
| `Bulge` | `double` | Facteur de courbure d'arc (0 = droit, ±1 = demi-cercle) |
| `TangentUsed` | `bool` | Si la direction tangente est utilisée |
| `Tangent` | `double` | Angle de direction tangente |

## Polyline vs Polyline2d

| Fonctionnalité | Polyline (Légère) | Polyline2d (Héritée) |
|----------------|-------------------|----------------------|
| Objets Base de Données | Objet unique | Conteneur + objets sommet |
| Utilisation Mémoire | Plus faible | Plus élevée |
| Performance | Plus rapide | Plus lent |
| XData par Sommet | Non | Oui (chaque sommet est un DBObject) |
| Support 3D | Non | Non (utiliser Polyline3d) |
| Recommandé | Oui (moderne) | Seulement pour compatibilité |

## Meilleures Pratiques

1. **Utiliser Polyligne Légère** : Préférez `Polyline` à `Polyline2d` pour les nouveaux dessins
2. **Gestion de Transaction** : N'oubliez pas d'ajouter à la fois la polyligne et les sommets à la transaction
3. **Propriété des Sommets** : Les sommets appartiennent à la polyligne et sont supprimés avec elle

## Objets Associés
- [Polyline](Polyline.md) - Polyligne 2D légère moderne
- [Polyline3d](Polyline3d.md) - Polyligne 3D
- [Curve](../../BaseClasses/Curve.md) - Classe de base pour les polylignes
- [Entity](../../BaseClasses/Entity.md) - Classe de base pour toutes les entités
- Vertex2d - Objet sommet individuel

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Polyline2d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Polyline2d)
- [Travailler avec les Polylignes](https://help.autodesk.com/view/OARX/2024/ENU/)
