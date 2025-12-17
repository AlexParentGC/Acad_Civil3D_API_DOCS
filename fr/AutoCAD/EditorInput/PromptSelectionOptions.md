# PromptSelectionOptions Class

## Vue d'Ensemble
La classe `PromptSelectionOptions` définit les options pour demander à l'utilisateur de sélectionner des entités dans AutoCAD. Elle fournit un contrôle étendu sur le comportement de sélection, y compris les mots-clés, les filtres et les modes de sélection.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Hiérarchie d'Héritage
```
System.Object
  └─ PromptOptions
      └─ PromptSelectionOptions
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Message` | `string` | Obtient ou définit le message d'invite affiché à l'utilisateur |
| `MessageForAdding` | `string` | Message affiché lors de l'ajout d'objets à la sélection |
| `MessageForRemoval` | `string` | Message affiché lors de la suppression d'objets de la sélection |
| `Keywords` | `PromptSelectionKeywordCollection` | Collection de mots-clés disponibles pendant la sélection |
| `KeywordInput` | `string` | Obtient le mot-clé entré par l'utilisateur |
| `AllowDuplicates` | `bool` | Permet de sélectionner le même objet plusieurs fois |
| `SingleOnly` | `bool` | Limite la sélection à un seul objet |
| `SinglePickInSpace` | `bool` | Permet une sélection unique dans l'espace modèle uniquement |
| `SelectEverythingInAperture` | `bool` | Sélectionne tous les objets dans la zone de sélection |
| `RejectObjectsFromNonCurrentSpace` | `bool` | Rejette les objets qui ne sont pas dans l'espace actuel |
| `RejectObjectsOnLockedLayers` | `bool` | Rejette les objets sur les calques verrouillés |
| `RejectPaperspaceViewport` | `bool` | Rejette les fenêtres de l'espace papier |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SetRejectMessage(string)` | `void` | Définit le message affiché lorsque la sélection est rejetée |
| `AddAllowedClass(Type, bool)` | `void` | Ajoute un type d'objet autorisé pour la sélection |

## Exemples de Code

### Exemple 1: Invite de Sélection de Base
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;

Document doc = Application.DocumentManager.MdiActiveDocument;
Editor ed = doc.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des objets : ";

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    SelectionSet selSet = result.Value;
    ed.WriteMessage($"\n{selSet.Count} objets sélectionnés");
}
```

### Exemple 2: Sélection avec Mots-Clés
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des objets ou [Tous/Fenêtre/Capture] : ";

// Ajouter des mots-clés
opts.Keywords.Add("Tous");
opts.Keywords.Add("Fenêtre");
opts.Keywords.Add("Capture");
opts.Keywords.Default = "Tous";

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} objets sélectionnés");
}
else if (result.Status == PromptStatus.Keyword)
{
    string keyword = result.StringResult;
    ed.WriteMessage($"\nMot-clé entré : {keyword}");
    
    if (keyword == "Tous")
    {
        // Sélectionner tous les objets
        result = ed.SelectAll();
    }
}
```

### Exemple 3: Sélection d'Objet Unique
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner un seul objet : ";
opts.SingleOnly = true;  // Limiter à une seule sélection
opts.SinglePickInSpace = true;  // Sélection unique dans l'espace modèle

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ObjectId[] ids = result.Value.GetObjectIds();
    ed.WriteMessage($"\nID de l'objet sélectionné : {ids[0]}");
}
```

### Exemple 4: Sélection avec Règles de Rejet
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des objets (calques déverrouillés uniquement) : ";
opts.RejectObjectsOnLockedLayers = true;
opts.SetRejectMessage("\nL'objet est sur un calque verrouillé !");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} objets sélectionnés sur des calques déverrouillés");
}
```

### Exemple 5: Sélection avec Filtrage de Type
```csharp
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner uniquement des lignes : ";

// Autoriser uniquement les objets Line
opts.AddAllowedClass(typeof(Line), true);
opts.SetRejectMessage("\nSeules les lignes peuvent être sélectionnées !");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{result.Value.Count} lignes sélectionnées");
}
```

### Exemple 6: Sélection Avancée avec Options Multiples
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des objets : ";
opts.MessageForAdding = "\nAjouter des objets : ";
opts.MessageForRemoval = "\nRetirer des objets : ";

// Configurer le comportement de sélection
opts.AllowDuplicates = false;
opts.RejectObjectsFromNonCurrentSpace = true;
opts.RejectPaperspaceViewport = true;
opts.SelectEverythingInAperture = false;

// Ajouter des mots-clés pour les modes de sélection
opts.Keywords.Add("Fenêtre");
opts.Keywords.Add("Capture");
opts.Keywords.Add("Barrière");
opts.Keywords.Add("Tous");

PromptSelectionResult result = ed.GetSelection(opts);

if (result.Status == PromptStatus.OK)
{
    SelectionSet selSet = result.Value;
    ed.WriteMessage($"\n{selSet.Count} objets uniques sélectionnés");
    
    // Traiter la sélection
    foreach (SelectedObject selObj in selSet)
    {
        ed.WriteMessage($"\nID de l'objet : {selObj.ObjectId}");
    }
}
else if (result.Status == PromptStatus.Keyword)
{
    string keyword = result.StringResult;
    ed.WriteMessage($"\nMode de sélection : {keyword}");
}
```

## Modèles Communs

### Sélection avec Annulation
```csharp
PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des objets : ";
opts.Keywords.Add("Annuler");

// Gérer l'annulation dans la boucle de sélection
```

### Combinaison avec SelectionFilter
```csharp
PromptSelectionOptions opts = new PromptSelectionOptions();
opts.Message = "\nSélectionner des cercles : ";

TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "CIRCLE")
};
SelectionFilter filter = new SelectionFilter(filterList);

PromptSelectionResult result = ed.GetSelection(opts, filter);
```

## Meilleures Pratiques

1. **Messages Clairs**: Fournir des messages d'invite clairs et descriptifs
2. **Mots-Clés**: Utiliser des mots-clés pour les modes de sélection courants (Tous, Fenêtre, Capture)
3. **Messages de Rejet**: Définir des messages de rejet personnalisés pour un meilleur retour utilisateur
4. **Sélection Unique**: Utiliser `SingleOnly` lorsqu'un seul objet est nécessaire
5. **Filtrage de Calque**: Utiliser `RejectObjectsOnLockedLayers` pour empêcher la sélection de calques verrouillés
6. **Filtrage de Type**: Combiner avec `SelectionFilter` pour un filtrage complexe
7. **Retour Utilisateur**: Toujours vérifier `PromptStatus` avant de traiter les résultats

## Classes Associées
- **PromptSelectionResult** - Résultat de l'invite de sélection
- **SelectionFilter** - Filtrer la sélection par propriétés d'entité
- **SelectionSet** - Collection d'objets sélectionnés
- **SelectedObject** - Objet sélectionné individuel
- **Editor** - Classe Editor avec méthodes GetSelection

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe PromptSelectionOptions](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_EditorInput_PromptSelectionOptions)
