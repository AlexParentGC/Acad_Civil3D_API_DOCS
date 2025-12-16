# Classe Editor

## Vue d'Ensemble
La classe `Editor` fournit des méthodes pour l'interaction utilisateur, l'entrée/sortie en ligne de commande, et la sélection d'entités dans AutoCAD.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Hiérarchie d'Héritage
```
System.Object
  └─ Editor
```

## Méthodes Clés - Sortie

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `WriteMessage(string)` | `void` | Écrit un message sur la ligne de commande |
| `WriteMessage(string, params object[])` | `void` | Écrit un message formaté |

## Méthodes Clés - Entrée

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetString(string)` | `PromptResult` | Invite pour une entrée chaîne |
| `GetInteger(string)` | `PromptIntegerResult` | Invite pour un entier |
| `GetDouble(string)` | `PromptDoubleResult` | Invite pour un double |
| `GetPoint(string)` | `PromptPointResult` | Invite pour un point |
| `GetDistance(string)` | `PromptDoubleResult` | Invite pour une distance |
| `GetAngle(string)` | `PromptDoubleResult` | Invite pour un angle |
| `GetKeywords(string, params string[])` | `PromptResult` | Invite pour un mot-clé |

## Méthodes Clés - Sélection

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetSelection()` | `PromptSelectionResult` | Invite pour la sélection d'entités |
| `GetSelection(SelectionFilter)` | `PromptSelectionResult` | Sélection avec filtre |
| `SelectAll(SelectionFilter)` | `PromptSelectionResult` | Sélectionne toutes les entités |

## Exemples de Code

### Exemple 1: Écrire sur la Ligne de Commande
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
Editor ed = acDoc.Editor;

ed.WriteMessage("\nBonjour depuis AutoCAD !");
ed.WriteMessage("\nFormaté : {0}, {1}", 123, "test");
```

### Exemple 2: Obtenir l'Entrée Utilisateur
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

// Obtenir une chaîne
PromptStringOptions pso = new PromptStringOptions("\nEntrez votre nom : ");
PromptResult pr = ed.GetString(pso);
if (pr.Status == PromptStatus.OK)
{
    string name = pr.StringResult;
    ed.WriteMessage($"\nBonjour, {name} !");
}

// Obtenir un entier
PromptIntegerOptions pio = new PromptIntegerOptions("\nEntrez un nombre : ");
pio.AllowNegative = false;
PromptIntegerResult pir = ed.GetInteger(pio);
if (pir.Status == PromptStatus.OK)
{
    int number = pir.Value;
}

// Obtenir un point
PromptPointOptions ppo = new PromptPointOptions("\nSélectionnez un point : ");
PromptPointResult ppr = ed.GetPoint(ppo);
if (ppr.Status == PromptStatus.OK)
{
    Point3d point = ppr.Value;
}
```

### Exemple 3: Sélection d'Entités
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptSelectionResult selResult = ed.GetSelection();

if (selResult.Status == PromptStatus.OK)
{
    SelectionSet selSet = selResult.Value;
    ed.WriteMessage($"\n{selSet.Count} objets sélectionnés");
    
    ObjectId[] ids = selSet.GetObjectIds();
    // Travailler avec les objets sélectionnés
}
```

### Exemple 4: Sélection Filtrée
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

// Créer un filtre pour les lignes seulement
TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "LINE")
};
SelectionFilter filter = new SelectionFilter(filterList);

PromptSelectionResult selResult = ed.GetSelection(filter);

if (selResult.Status == PromptStatus.OK)
{
    ed.WriteMessage($"\n{selResult.Value.Count} lignes sélectionnées");
}
```

### Exemple 5: Entrée par Mot-Clé
```csharp
using Autodesk.AutoCAD.EditorInput;

Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;

PromptKeywordOptions pko = new PromptKeywordOptions("\nChoisir option [Oui/Non] : ");
pko.Keywords.Add("Oui");
pko.Keywords.Add("Non");
pko.Keywords.Default = "Oui";

PromptResult pr = ed.GetKeyword(pko);

if (pr.Status == PromptStatus.OK)
{
    string keyword = pr.StringResult;
    ed.WriteMessage($"\nVous avez choisi : {keyword}");
}
```

## Valeurs PromptStatus

| Statut | Description |
|--------|-------------|
| `OK` | L'utilisateur a fourni une entrée valide |
| `Cancel` | L'utilisateur a annulé (ESC) |
| `Error` | Une erreur est survenue |
| `None` | Pas d'entrée |
| `Keyword` | Un mot-clé a été entré |

## Objets Associés
- [Document](Document.md) - Contient l'Éditeur
- [SelectionSet](SelectionSet.md) - Entités sélectionnées
- [SelectionFilter](SelectionFilter.md) - Filtrage d'entités

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_EditorInput_Editor)
