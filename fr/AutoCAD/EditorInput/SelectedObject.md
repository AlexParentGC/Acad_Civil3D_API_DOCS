# SelectedObject Class

## Vue d'Ensemble
Représente un objet sélectionné par l'utilisateur lors d'une opération de sélection.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `ObjectId` - ObjectId de l'entité sélectionnée
- `SelectionMethod` - Comment l'objet a été sélectionné

## Exemple de Code
```csharp
PromptSelectionResult result = ed.GetSelection();
if (result.Status == PromptStatus.OK)
{
    foreach (SelectedObject selObj in result.Value)
    {
        ObjectId id = selObj.ObjectId;
        // Traiter l'objet
    }
}
```

## Classes Associées
- SelectionSet, PromptSelectionResult

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
