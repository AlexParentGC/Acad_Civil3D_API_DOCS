# SelectionFilter Class

## Vue d'Ensemble
La classe `SelectionFilter` filtre la sélection d'entités en fonction des codes DXF et des valeurs. Essentielle pour sélectionner des types d'entités spécifiques, des calques, des couleurs et d'autres propriétés.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Constructeur
```csharp
SelectionFilter(TypedValue[] filterList)
```

## Exemple de Code
```csharp
using Autodesk.AutoCAD.EditorInput;

// Filtre pour les lignes uniquement
TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "LINE")
};
SelectionFilter filter = new SelectionFilter(filterList);

PromptSelectionResult result = ed.GetSelection(filter);
```

## Filtres Communs
- **Type d'Entité:** `DxfCode.Start` + nom d'entité ("LINE", "CIRCLE", etc.)
- **Calque:** `DxfCode.LayerName` + nom du calque
- **Couleur:** `DxfCode.Color` + index de couleur
- **Type de Ligne:** `DxfCode.LinetypeName` + nom du type de ligne

## Classes Associées
- **TypedValue** - Paires code/valeur DXF
- **PromptSelectionOptions** - Options de sélection
- **Editor** - Méthodes GetSelection

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
