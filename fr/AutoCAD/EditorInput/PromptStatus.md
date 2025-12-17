# PromptStatus Enumeration

## Vue d'Ensemble
Énumération définissant le statut d'une opération d'invite.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Valeurs

| Valeur | Description |
|--------|-------------|
| `OK` | L'utilisateur a fourni une entrée valide |
| `Cancel` | L'utilisateur a annulé (ESC) |
| `Error` | Une erreur s'est produite |
| `None` | Aucune entrée fournie |
| `Keyword` | Un mot-clé a été entré |
| `Modeless` | Opération sans mode |
| `Other` | Autre statut |

## Exemple de Code
```csharp
PromptSelectionResult result = ed.GetSelection();
if (result.Status == PromptStatus.OK)
{
    // Traiter la sélection
}
else if (result.Status == PromptStatus.Cancel)
{
    ed.WriteMessage("\nAnnulé");
}
```

## Classes Associées
- Toutes les classes de résultats d'invite

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
