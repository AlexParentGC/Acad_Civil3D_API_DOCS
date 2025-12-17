# PromptSelectionResult Class

## Vue d'Ensemble
La classe `PromptSelectionResult` représente le résultat d'une opération d'invite de sélection. Elle contient le jeu de sélection et les informations de statut sur la réponse de l'utilisateur.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Hiérarchie d'Héritage
```
System.Object
  └─ PromptResult
      └─ PromptSelectionResult
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Status` | `PromptStatus` | Obtient le statut de l'invite (OK, Cancel, Error, etc.) |
| `Value` | `SelectionSet` | Obtient le jeu de sélection si Status est OK |
| `StringResult` | `string` | Obtient la chaîne de mot-clé si un mot-clé a été entré |

## Exemples de Code

### Exemple 1: Gestion de Résultat de Sélection de Base
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.DatabaseServices;

Document doc = Application.DocumentManager.MdiActiveDocument;
Editor ed = doc.Editor;

PromptSelectionResult result = ed.GetSelection();

if (result.Status == PromptStatus.OK)
{
    SelectionSet selSet = result.Value;
    ed.WriteMessage($"\n{selSet.Count} objets sélectionnés");
    
    // Obtenir les IDs d'objets
    ObjectId[] ids = selSet.GetObjectIds();
    
    // Traiter les objets sélectionnés
    using (Transaction tr = doc.TransactionManager.StartTransaction())
    {
        foreach (ObjectId id in ids)
        {
            Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
            ed.WriteMessage($"\nType d'objet : {ent.GetType().Name}");
        }
        tr.Commit();
    }
}
else if (result.Status == PromptStatus.Cancel)
{
    ed.WriteMessage("\nSélection annulée par l'utilisateur");
}
else if (result.Status == PromptStatus.Error)
{
    ed.WriteMessage("\nErreur lors de la sélection");
}
```

### Exemple 2: Gestion des Mots-Clés dans la Sélection
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des objets ou [Tous] : ";
opts.Keywords.Add("Tous");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} objets sélectionnés");
}
else if (result.Status == PromptStatus.Keyword)
{
    string keyword = result.StringResult;
    if (keyword == "Tous")
    {
        // Sélectionner tous les objets
        result = ed.SelectAll();
        if (result.Status == PromptStatus.OK)
        {
            ed.WriteMessage($"\nTous les objets sélectionnés : {result.Value.Count}");
        }
    }
}
```

### Exemple 3: Traitement du Jeu de Sélection
```csharp
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.DatabaseServices;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
Database db = ed.Document.Database;

PromptSelectionResult result = ed.GetSelection();

if (result.Status == PromptStatus.OK)
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        foreach (SelectedObject selObj in result.Value)
        {
            if (selObj != null)
            {
                Entity ent = tr.GetObject(selObj.ObjectId, OpenMode.ForWrite) as Entity;
                
                // Modifier l'entité
                ent.ColorIndex = 1; // Rouge
                
                ed.WriteMessage($"\nModifié : {ent.GetType().Name}");
            }
        }
        tr.Commit();
    }
}
```

## Meilleures Pratiques

1. **Toujours Vérifier le Statut**: Vérifier la propriété `Status` avant d'accéder à `Value`
2. **Vérifications Null**: Le jeu de sélection peut être null si le statut n'est pas OK
3. **Transaction**: Utiliser des transactions lors de la modification d'objets sélectionnés
4. **Gestion des Mots-Clés**: Vérifier `PromptStatus.Keyword` lors de l'utilisation de mots-clés
5. **Gestion des Erreurs**: Gérer les statuts Cancel et Error de manière appropriée

## Classes Associées
- **PromptSelectionOptions** - Options pour l'invite de sélection
- **SelectionSet** - Collection d'objets sélectionnés
- **SelectedObject** - Objet sélectionné individuel
- **PromptStatus** - Énumération de statut
- **Editor** - Classe Editor avec méthodes GetSelection

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe PromptSelectionResult](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_EditorInput_PromptSelectionResult)
