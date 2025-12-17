# Layout

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `Layout` représente une présentation (espace papier ou espace modèle) dans un dessin AutoCAD. Les présentations définissent la taille du papier, les paramètres de traçage et les fenêtres pour organiser et tracer les dessins.

**Concept Clé:** Chaque dessin a au moins deux présentations : l'espace modèle et une ou plusieurs présentations d'espace papier. Chaque présentation peut avoir ses propres paramètres de traçage et fenêtres.

## Hiérarchie de Classe

```
Object
  └─ RXObject
      └─ DBObject
          └─ PlotSettings
              └─ Layout
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `LayoutName` | `string` | Obtient/définit le nom de la présentation |
| `TabOrder` | `int` | Obtient/définit l'ordre d'affichage des onglets |
| `TabSelected` | `bool` | Obtient/définit si l'onglet de présentation est sélectionné |
| `BlockTableRecordId` | `ObjectId` | Obtient l'enregistrement de table de blocs associé |
| `ModelType` | `bool` | Vérifie s'il s'agit de la présentation Modèle |

## Exemples de Code

### Liste de Toutes les Présentations

```csharp
using Autodesk.AutoCAD.DatabaseServices;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
        OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\n=== Présentations ===");
    
    foreach (DBDictionaryEntry entry in layoutDict)
    {
        Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
        
        ed.WriteMessage($"\n{layout.LayoutName}");
        ed.WriteMessage($"\n  Ordre Onglet : {layout.TabOrder}");
        ed.WriteMessage($"\n  Type Modèle : {layout.ModelType}");
    }
    
    tr.Commit();
}
```

### Création d'une Nouvelle Présentation

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
        OpenMode.ForWrite) as DBDictionary;
    
    // Créer une nouvelle présentation
    Layout layout = new Layout();
    layout.LayoutName = "Nouvelle_Présentation";
    layout.AddToLayoutDictionary(db, layoutDict.ObjectId);
    
    tr.AddNewlyCreatedDBObject(layout, true);
    
    ed.WriteMessage($"\nPrésentation '{layout.LayoutName}' créée");
    tr.Commit();
}
```

### Basculer vers une Présentation

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, 
        OpenMode.ForRead) as DBDictionary;
    
    Layout layout = tr.GetObject(layoutDict.GetAt("Nom_Présentation"), 
        OpenMode.ForWrite) as Layout;
    
    LayoutManager.Current.CurrentLayout = layout.LayoutName;
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Ne Pas Supprimer Modèle**: Ne jamais supprimer la présentation Modèle
2. **Noms Uniques**: S'assurer que les noms de présentation sont uniques
3. **Utiliser LayoutManager**: Préférer les méthodes LayoutManager pour les opérations
4. **Ordre des Onglets**: Définir un ordre logique des onglets pour la navigation utilisateur
5. **Paramètres de Traçage**: Configurer les paramètres de traçage par présentation

## Classes Associées

- [PlotSettings](PlotSettings.md) - Classe de base pour la configuration de traçage
- LayoutManager - Gère les opérations de présentation
- Viewport - Entité de fenêtre de présentation
- [BlockTableRecord](../SymbolTables/BlockTable.md) - Contient les entités de présentation

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
