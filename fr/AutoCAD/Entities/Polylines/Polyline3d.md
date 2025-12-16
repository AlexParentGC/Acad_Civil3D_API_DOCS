# Classe Polyline3d

## Vue d'Ensemble
La classe `Polyline3d` représente une entité polyligne 3D dans AutoCAD. Similaire à `Polyline2d`, c'est un objet conteneur qui possède une collection d'objets `PolylineVertex3d`. Chaque sommet peut avoir une position 3D unique, ce qui la rend adaptée à la représentation de chemins 3D, de pipelines et de courbes spatiales.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Polyline3d
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `PolyType` | `Poly3dType` | Obtient/définit le type de polyligne (SimplePoly, QuadSplinePoly, CubicSplinePoly) |
| `Closed` | `bool` | Obtient/définit si la polyligne est fermée |
| `Length` | `double` | Obtient la longueur totale de la polyligne |
| `StartPoint` | `Point3d` | Obtient le point de départ de la polyligne |
| `EndPoint` | `Point3d` | Obtient le point de fin de la polyligne |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetEnumerator()` | `IEnumerator` | Obtient un énumérateur pour les sommets |
| `AppendVertex(PolylineVertex3d)` | `void` | Ajoute un sommet à la polyligne |
| `InsertVertexAt(int, PolylineVertex3d)` | `void` | Insère un sommet à l'index spécifié |
| `OpenVertex(PolylineVertex3d, OpenMode)` | `PolylineVertex3d` | Ouvre un sommet en lecture ou écriture |
| `ConvertToPolylineType(Poly3dType)` | `void` | Convertit la polyligne en un type différent |
| `Straighten()` | `void` | Supprime tout lissage de courbe |
| `SplineFit()` | `void` | Applique un lissage spline |

## Exemples de Code

### Exemple 1: Créer une Polyligne 3D
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer le conteneur de polyligne 3D
    Polyline3d poly = new Polyline3d();
    poly.SetDatabaseDefaults();
    
    // Ajouter la polyligne au dessin
    ObjectId polyId = btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Créer les sommets 3D
    Point3d[] points = new Point3d[]
    {
        new Point3d(0, 0, 0),
        new Point3d(10, 0, 5),
        new Point3d(10, 10, 10),
        new Point3d(0, 10, 15),
        new Point3d(0, 0, 20)
    };
    
    foreach (Point3d pt in points)
    {
        PolylineVertex3d vertex = new PolylineVertex3d(pt);
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    tr.Commit();
}
```

### Exemple 2: Lire les Sommets de Polyligne 3D
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline3d;
    
    ed.WriteMessage($"\n=== Information Polyline3d ===");
    ed.WriteMessage($"\nType : {poly.PolyType}");
    ed.WriteMessage($"\nFermée : {poly.Closed}");
    ed.WriteMessage($"\nLongueur : {poly.Length:F3}");
    ed.WriteMessage($"\nPoint de Départ : {poly.StartPoint}");
    ed.WriteMessage($"\nPoint de Fin : {poly.EndPoint}");
    
    ed.WriteMessage("\n\nSommets :");
    int index = 0;
    
    foreach (ObjectId vertexId in poly)
    {
        PolylineVertex3d vertex = tr.GetObject(vertexId, OpenMode.ForRead) as PolylineVertex3d;
        
        Point3d pos = vertex.Position;
        ed.WriteMessage($"\n  Sommet {index} : ({pos.X:F2}, {pos.Y:F2}, {pos.Z:F2})");
        ed.WriteMessage($"\n    Type Sommet : {vertex.VertexType}");
        
        index++;
    }
    
    tr.Commit();
}
```

## Types de Polyligne 3D

| Type | Description |
|------|-------------|
| `SimplePoly` | Polyligne 3D standard avec segments droits |
| `QuadSplinePoly` | B-spline quadratique passant par les sommets |
| `CubicSplinePoly` | B-spline cubique passant par les sommets |

## Propriétés PolylineVertex3d

| Propriété | Type | Description |
|-----------|------|-------------|
| `Position` | `Point3d` | Emplacement du sommet 3D |
| `VertexType` | `Vertex3dType` | Type de sommet (simple, contrôle spline, lissage spline) |

## Polyline3d vs Polyline

| Fonctionnalité | Polyline (Légère) | Polyline3d |
|----------------|-------------------|------------|
| Dimensions | 2D seulement | 3D complète |
| Objets Base de Données | Objet unique | Conteneur + objets sommet |
| Support Largeur | Oui (par segment) | Non |
| Courbure/Arcs | Oui | Non |
| Lissage Spline | Non | Oui |
| Cas d'Utilisation | Dessins 2D | Chemins 3D, tuyaux, câbles |

## Meilleures Pratiques

1. **Chemins 3D** : Utilisez Polyline3d pour les vrais chemins spatiaux 3D
2. **Gestion de Transaction** : Ajoutez à la fois la polyligne et tous les sommets à la transaction
3. **Propriété des Sommets** : Les sommets appartiennent à la polyligne et sont supprimés avec elle
4. **Performance** : Pour de nombreux sommets, envisagez d'utiliser une spline 3D à la place
5. **Polylignes Fermées** : Définissez `Closed = true` pour connecter les extrémités
6. **Lissage Spline** : Utilisez pour des courbes douces à travers les points de contrôle
7. **Coordonnées Z** : Chaque sommet peut avoir une valeur Z unique
8. **Visualisation** : Définissez la couleur et le type de ligne appropriés pour la visibilité 3D

## Objets Associés
- [Polyline](Polyline.md) - Polyligne 2D légère
- [Polyline2d](Polyline2d.md) - Polyligne 2D héritée
- [Curve](../../BaseClasses/Curve.md) - Classe de base pour les polylignes
- [Entity](../../BaseClasses/Entity.md) - Classe de base pour toutes les entités
- PolylineVertex3d - Objet sommet 3D individuel
- [Spline](../Geometric/Spline.md) - Alternative pour les courbes 3D douces

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Polyline3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Polyline3d)
- [Travailler avec les Polylignes 3D](https://help.autodesk.com/view/OARX/2024/ENU/)
