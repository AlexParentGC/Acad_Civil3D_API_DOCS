# Classe Leader

## Vue d'Ensemble
La classe `Leader` représente une entité d'annotation ligne de repère dans AutoCAD. Une ligne de repère est une ligne ou une spline avec une pointe de flèche qui connecte un texte d'annotation ou des blocs à des caractéristiques spécifiques dans un dessin. Les lignes de repère sont couramment utilisées pour les étiquettes, notes et annotations dimensionnelles.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Leader
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `HasArrowHead` | `bool` | Obtient/définit si le repère a une pointe de flèche |
| `HasHookLine` | `bool` | Obtient/définit si le repère a une ligne de crochet |
| `IsSplined` | `bool` | Obtient/définit si le repère est une spline |
| `Annotation` | `ObjectId` | Obtient/définit l'objet d'annotation (texte, bloc, etc.) |
| `DimStyle` | `ObjectId` | Obtient/définit le style de dimension |
| `DimScale` | `double` | Obtient/définit l'échelle de dimension |
| `ColorIndex` | `short` | Obtient/définit l'index de couleur |
| `NumVertices` | `int` | Obtient le nombre de sommets |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `AppendVertex(Point3d)` | `void` | Ajoute un sommet au repère |
| `GetVertexAt(int)` | `Point3d` | Obtient le sommet à l'index spécifié |
| `SetVertexAt(int, Point3d)` | `void` | Définit le sommet à l'index spécifié |
| `RemoveLastVertex()` | `void` | Supprime le dernier sommet |
| `EvaluateLeader()` | `void` | Recalcule la géométrie du repère |
| `AttachAnnotation(ObjectId)` | `void` | Attache une annotation au repère |

## Exemples de Code

### Exemple 1: Créer un Repère Simple
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer le repère
    Leader leader = new Leader();
    leader.SetDatabaseDefaults();
    
    // Ajouter des sommets (du point d'annotation vers l'élément)
    leader.AppendVertex(new Point3d(10, 10, 0)); // Point d'annotation
    leader.AppendVertex(new Point3d(5, 5, 0));   // Point coudé
    leader.AppendVertex(new Point3d(0, 0, 0));   // Point élément (flèche)
    
    // Définir les propriétés
    leader.HasArrowHead = true;
    leader.ColorIndex = 1; // Rouge
    
    // Ajouter au dessin
    btr.AppendEntity(leader);
    tr.AddNewlyCreatedDBObject(leader, true);
    
    // Évaluer pour mettre à jour la géométrie
    leader.EvaluateLeader();
    
    tr.Commit();
}
```

### Exemple 2: Créer un Repère avec Annotation Texte
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer l'annotation texte
    MText mtext = new MText();
    mtext.SetDatabaseDefaults();
    mtext.Location = new Point3d(10, 10, 0);
    mtext.Contents = "Ceci est une note";
    mtext.TextHeight = 2.5;
    
    ObjectId mtextId = btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    // Créer le repère
    Leader leader = new Leader();
    leader.SetDatabaseDefaults();
    leader.AppendVertex(new Point3d(10, 10, 0));
    leader.AppendVertex(new Point3d(5, 5, 0));
    leader.AppendVertex(new Point3d(0, 0, 0));
    leader.HasArrowHead = true;
    
    // Attacher l'annotation
    leader.Annotation = mtextId;
    
    btr.AppendEntity(leader);
    tr.AddNewlyCreatedDBObject(leader, true);
    leader.EvaluateLeader();
    
    tr.Commit();
}
```

### Exemple 3: Créer un Repère Spline
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Leader leader = new Leader();
    leader.SetDatabaseDefaults();
    
    // Ajouter plusieurs sommets pour une courbe lisse
    leader.AppendVertex(new Point3d(15, 15, 0));
    leader.AppendVertex(new Point3d(12, 10, 0));
    leader.AppendVertex(new Point3d(8, 8, 0));
    leader.AppendVertex(new Point3d(5, 5, 0));
    leader.AppendVertex(new Point3d(0, 0, 0));
    
    // Rendre en spline
    leader.IsSplined = true;
    leader.HasArrowHead = true;
    leader.ColorIndex = 3; // Vert
    
    btr.AppendEntity(leader);
    tr.AddNewlyCreatedDBObject(leader, true);
    leader.EvaluateLeader();
    
    tr.Commit();
}
```

### Exemple 4: Lire les Propriétés du Repère
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Leader leader = tr.GetObject(leaderId, OpenMode.ForRead) as Leader;
    
    ed.WriteMessage("\n=== Information Leader ===");
    ed.WriteMessage($"\nA une Flèche : {leader.HasArrowHead}");
    ed.WriteMessage($"\nA une Ligne Crochet : {leader.HasHookLine}");
    ed.WriteMessage($"\nEst Spline : {leader.IsSplined}");
    ed.WriteMessage($"\nNombre de Sommets : {leader.NumVertices}");
    ed.WriteMessage($"\nAnnotation : {leader.Annotation}");
    
    ed.WriteMessage("\n\nSommets :");
    for (int i = 0; i < leader.NumVertices; i++)
    {
        Point3d vertex = leader.GetVertexAt(i);
        ed.WriteMessage($"\n  Sommet {i} : ({vertex.X:F2}, {vertex.Y:F2}, {vertex.Z:F2})");
    }
    
    tr.Commit();
}
```

### Exemple 5: Modifier les Sommets du Repère
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Leader leader = tr.GetObject(leaderId, OpenMode.ForWrite) as Leader;
    
    // Déplacer le sommet du milieu
    if (leader.NumVertices >= 3)
    {
        Point3d middleVertex = leader.GetVertexAt(1);
        Point3d newVertex = new Point3d(middleVertex.X + 5, middleVertex.Y + 5, middleVertex.Z);
        leader.SetVertexAt(1, newVertex);
        
        leader.EvaluateLeader();
    }
    
    tr.Commit();
}
```

### Exemple 6: Repère avec Annotation Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer une référence de bloc (en supposant que le bloc "DetailCallout" existe)
    if (bt.Has("DetailCallout"))
    {
        BlockReference blockRef = new BlockReference(new Point3d(10, 10, 0), bt["DetailCallout"]);
        blockRef.SetDatabaseDefaults();
        
        ObjectId blockRefId = btr.AppendEntity(blockRef);
        tr.AddNewlyCreatedDBObject(blockRef, true);
        
        // Créer le repère
        Leader leader = new Leader();
        leader.SetDatabaseDefaults();
        leader.AppendVertex(new Point3d(10, 10, 0));
        leader.AppendVertex(new Point3d(5, 5, 0));
        leader.AppendVertex(new Point3d(0, 0, 0));
        leader.HasArrowHead = true;
        leader.Annotation = blockRefId;
        
        btr.AppendEntity(leader);
        tr.AddNewlyCreatedDBObject(leader, true);
        leader.EvaluateLeader();
    }
    
    tr.Commit();
}
```

### Exemple 7: Définir le Style de Dimension du Repère
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Leader leader = tr.GetObject(leaderId, OpenMode.ForWrite) as Leader;
    
    // Obtenir la table de style de dimension
    DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
    
    if (dst.Has("Annotative"))
    {
        leader.DimStyle = dst["Annotative"];
        leader.EvaluateLeader();
        
        ed.WriteMessage("\nStyle de dimension 'Annotative' appliqué");
    }
    
    tr.Commit();
}
```

### Exemple 8: Créer Plusieurs Repères
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d centerPoint = new Point3d(0, 0, 0);
    int numLeaders = 8;
    double radius = 10.0;
    
    for (int i = 0; i < numLeaders; i++)
    {
        double angle = (i * 2 * Math.PI) / numLeaders;
        Point3d endPoint = new Point3d(
            centerPoint.X + radius * Math.Cos(angle),
            centerPoint.Y + radius * Math.Sin(angle),
            0
        );
        
        Leader leader = new Leader();
        leader.SetDatabaseDefaults();
        leader.AppendVertex(endPoint);
        leader.AppendVertex(new Point3d(
            centerPoint.X + (radius * 0.5) * Math.Cos(angle),
            centerPoint.Y + (radius * 0.5) * Math.Sin(angle),
            0
        ));
        leader.AppendVertex(centerPoint);
        leader.HasArrowHead = true;
        leader.ColorIndex = (short)(i % 7 + 1);
        
        btr.AppendEntity(leader);
        tr.AddNewlyCreatedDBObject(leader, true);
        leader.EvaluateLeader();
    }
    
    ed.WriteMessage($"\nCréé {numLeaders} repères");
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Appeler EvaluateLeader()** : Toujours appeler après avoir créé ou modifié un repère
2. **Ordre des Sommets** : Le premier sommet est le point d'annotation, le dernier est le point de flèche
3. **Minimum de Sommets** : Besoin d'au moins 2 sommets pour un repère valide
4. **Attachement d'Annotation** : Attachez l'annotation pour un positionnement automatique
5. **Styles de Dimension** : Utilisez les styles de dimension pour une apparence cohérente
6. **Repères Spline** : Utilisez pour des repères courbés et d'aspect organique
7. **Lignes de Crochet** : Activez pour une ligne d'atterrissage horizontale à l'annotation
8. **Couleur et Type de Ligne** : Définissez explicitement pour la visibilité

## Leader vs MLeader

| Fonctionnalité | Leader | MLeader |
|----------------|--------|---------|
| Introduction | Legacy (R13+) | Moderne (2008+) |
| Flexibilité | Basique | Avancée |
| Repères Multiples | Non | Oui |
| Types de Contenu | Limité | Texte, Bloc, Aucun |
| Styles | Style de Dimension | Style MLeader |
| Recommandé | Compatibilité héritée | Nouveaux dessins |

## Cas d'Utilisation Courants

- **Étiquettes** : Identification de caractéristiques dans les dessins
- **Notes** : Ajout de texte explicatif
- **Références de Détail** : Pointant vers des vues de détail
- **Annotations Dimensionnelles** : Informations dimensionnelles supplémentaires
- **Étiquettes de Pièce** : Identification de composants

## Objets Associés
- [MLeader](MLeader.md) - Annotation multi-repère moderne
- [Dimension](Dimension.md) - Classe de base d'annotation de dimension
- [Curve](../../BaseClasses/Curve.md) - Classe de base pour Leader
- [Entity](../../BaseClasses/Entity.md) - Classe de base pour toutes les entités
- MText - Annotation texte pour repères
- BlockReference - Annotation bloc pour repères

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe Leader](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Leader)
