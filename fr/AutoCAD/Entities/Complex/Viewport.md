# Classe Viewport

## Vue d'Ensemble
La classe `Viewport` représente une entité fenêtre de présentation dans AutoCAD. Les fenêtres sont des vues dans les présentations de l'espace papier qui affichent des vues de la géométrie de l'espace objet. Elles vous permettent de créer plusieurs vues de votre modèle à différentes échelles, orientations et paramètres de visibilité de calque sur une seule feuille de présentation.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Viewport
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ViewCenter` | `Point2d` | Obtient/définit le point central de la vue |
| `ViewTarget` | `Point3d` | Obtient/définit le point cible de la vue dans l'espace objet |
| `ViewDirection` | `Vector3d` | Obtient/définit la direction de vue |
| `ViewHeight` | `double` | Obtient/définit la hauteur de vue en unités de l'espace objet |
| `CustomScale` | `double` | Obtient/définit l'échelle personnalisée de la fenêtre |
| `StandardScale` | `StandardScaleType` | Obtient/définit le type d'échelle standard |
| `Width` | `double` | Obtient/définit la largeur de la fenêtre |
| `Height` | `double` | Obtient/définit la hauteur de la fenêtre |
| `CenterPoint` | `Point3d` | Obtient/définit le centre de la fenêtre dans l'espace papier |
| `On` | `bool` | Obtient/définit si la fenêtre est active |
| `Locked` | `bool` | Obtient/définit si la fenêtre est verrouillée |
| `NonRectClipEntityId` | `ObjectId` | Obtient/définit le contour de délimitation non rectangulaire |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SyncModelView()` | `void` | Synchronise la fenêtre avec la vue de l'espace objet |
| `GetFrozenLayerList()` | `ObjectIdCollection` | Obtient les calques gelés dans cette fenêtre |
| `FreezeLayersInViewport(ObjectIdCollection)` | `void` | Gèle les calques dans cette fenêtre |
| `ThawLayersInViewport(ObjectIdCollection)` | `void` | Dégèle les calques dans cette fenêtre |
| `IsLayerFrozenInViewport(ObjectId)` | `bool` | Vérifie si un calque est gelé dans la fenêtre |

## Exemples de Code

### Exemple 1: Créer une Fenêtre Simple
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir une présentation (pas Modèle)
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    Layout layout = tr.GetObject(layoutDict.GetAt("Layout1"), OpenMode.ForRead) as Layout;
    
    BlockTableRecord layoutBtr = tr.GetObject(layout.BlockTableRecordId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer la fenêtre
    Viewport viewport = new Viewport();
    viewport.SetDatabaseDefaults();
    
    // Définir position et taille dans l'espace papier
    viewport.CenterPoint = new Point3d(150, 100, 0); // Unités espace papier (mm)
    viewport.Width = 200;
    viewport.Height = 150;
    
    // Définir les propriétés de vue
    viewport.ViewCenter = new Point2d(0, 0); // Centre espace objet
    viewport.ViewHeight = 100; // Hauteur espace objet
    
    // Activer la fenêtre
    viewport.On = true;
    
    // Ajouter à la présentation
    layoutBtr.AppendEntity(viewport);
    tr.AddNewlyCreatedDBObject(viewport, true);
    
    tr.Commit();
}
```

### Exemple 2: Définir l'Échelle de la Fenêtre
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    
    // Définir échelle standard (1:50)
    viewport.StandardScale = StandardScaleType.Scale1To50;
    
    // Ou définir échelle personnalisée
    // viewport.CustomScale = 50.0; // échelle 1:50
    
    ed.WriteMessage($"\nÉchelle fenêtre définie à 1:50");
    ed.WriteMessage($"\nHauteur vue : {viewport.ViewHeight:F2}");
    
    tr.Commit();
}
```

### Exemple 3: Geler des Calques dans la Fenêtre
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    // Obtenir les calques à geler
    ObjectIdCollection layersToFreeze = new ObjectIdCollection();
    
    if (lt.Has("Dimensions"))
        layersToFreeze.Add(lt["Dimensions"]);
    if (lt.Has("Text"))
        layersToFreeze.Add(lt["Text"]);
    
    // Geler les calques dans cette fenêtre uniquement
    viewport.FreezeLayersInViewport(layersToFreeze);
    
    ed.WriteMessage($"\nGelé {layersToFreeze.Count} calques dans la fenêtre");
    
    tr.Commit();
}
```

### Exemple 4: Lire les Propriétés de la Fenêtre
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForRead) as Viewport;
    
    ed.WriteMessage("\n=== Information Fenêtre ===");
    ed.WriteMessage($"\nActivée : {viewport.On}");
    ed.WriteMessage($"\nVerrouillée : {viewport.Locked}");
    ed.WriteMessage($"\nPoint Centre : {viewport.CenterPoint}");
    ed.WriteMessage($"\nLargeur : {viewport.Width:F2}");
    ed.WriteMessage($"\nHauteur : {viewport.Height:F2}");
    ed.WriteMessage($"\nCentre Vue : {viewport.ViewCenter}");
    ed.WriteMessage($"\nHauteur Vue : {viewport.ViewHeight:F2}");
    ed.WriteMessage($"\nCible Vue : {viewport.ViewTarget}");
    ed.WriteMessage($"\nDirection Vue : {viewport.ViewDirection}");
    ed.WriteMessage($"\nÉchelle Personnalisée : {viewport.CustomScale:F2}");
    ed.WriteMessage($"\nÉchelle Standard : {viewport.StandardScale}");
    
    // Lister les calques gelés
    ObjectIdCollection frozenLayers = viewport.GetFrozenLayerList();
    ed.WriteMessage($"\n\nCalques Gelés : {frozenLayers.Count}");
    
    foreach (ObjectId layerId in frozenLayers)
    {
        LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
        ed.WriteMessage($"\n  - {ltr.Name}");
    }
    
    tr.Commit();
}
```

### Exemple 5: Verrouiller/Déverrouiller la Fenêtre
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Viewport viewport = tr.GetObject(viewportId, OpenMode.ForWrite) as Viewport;
    
    // Verrouiller la fenêtre pour empêcher pan/zoom accidentel
    viewport.Locked = true;
    
    ed.WriteMessage($"\nFenêtre verrouillée : {viewport.Locked}");
    
    tr.Commit();
}
```

## Types d'Échelle Standard

Les échelles standard courantes incluent :
- `Scale1To1` - 1:1 (taille réelle)
- `Scale1To2` - 1:2
- `Scale1To4` - 1:4
- `Scale1To5` - 1:5
- `Scale1To8` - 1:8
- `Scale1To10` - 1:10
- `Scale1To16` - 1:16
- `Scale1To20` - 1:20
- `Scale1To50` - 1:50
- `Scale1To100` - 1:100

## États de la Fenêtre

| Propriété | Description |
|-----------|-------------|
| `On = true` | La fenêtre affiche le contenu de l'espace objet |
| `On = false` | La fenêtre est vide (off) |
| `Locked = true` | Pan/zoom verrouillé, empêche les changements accidentels |
| `Locked = false` | Pan/zoom activé |

## Meilleures Pratiques

1. **Verrouiller les Fenêtres** : Verrouillez les fenêtres après configuration pour éviter les changements accidentels
2. **Gel de Calque** : Utilisez le gel de calque spécifique à la fenêtre pour différentes vues
3. **Échelles Standard** : Utilisez des échelles standard pour la cohérence
4. **Arrangement des Fenêtres** : Organisez les fenêtres logiquement sur les présentations
5. **Désactiver Si Non Nécessaire** : Désactivez les fenêtres pour améliorer les performances

## Objets Associés
- [Entity](../../BaseClasses/Entity.md) - Classe de base pour Viewport
- Layout - Présentation espace papier contenant les fenêtres
- [Database](../../Core/Database.md) - Contient le dictionnaire de présentation
- LayerTableRecord - Calques pouvant être gelés par fenêtre

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Viewport)
