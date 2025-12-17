# SelectedSubObject Class

## Vue d'Ensemble
Représente une sous-entité sélectionnée (face, arête, sommet) d'un solide ou d'une surface.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `ObjectId` - ID de l'objet parent
- `FullSubentityPath` - Chemin vers la sous-entité
- `GraphicsSystemMarker` - Marqueur du système graphique

## Exemple de Code
```csharp
// Sélection de sous-entité pour solides/surfaces
PromptSelectionResult result = ed.GetSelection();
if (result.Status == PromptStatus.OK)
{
    foreach (SelectedObject selObj in result.Value)
    {
        if (selObj is SelectedSubObject subObj)
        {
            // Traiter la sous-entité
        }
    }
}
```

## Classes Associées
- SelectedObject, FullSubentityPath

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
