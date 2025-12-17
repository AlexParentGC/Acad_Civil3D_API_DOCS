# TypedValue Structure

## Vue d'Ensemble
Représente une paire de code DXF et de valeur utilisée pour le filtrage d'entités et les données étendues.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Constructeur
```csharp
TypedValue(int typeCode, object value)
```

## Propriétés Clés
- `TypeCode` - Code DXF (int)
- `Value` - Valeur associée (object)

## Exemple de Code
```csharp
// Filtre pour les lignes sur le calque "0"
TypedValue[] filterList = new TypedValue[]
{
    new TypedValue((int)DxfCode.Start, "LINE"),
    new TypedValue((int)DxfCode.LayerName, "0")
};
SelectionFilter filter = new SelectionFilter(filterList);
```

## Codes DXF Communs
- `0` (Start) - Type d'entité
- `8` (LayerName) - Nom du calque
- `62` (Color) - Index de couleur
- `6` (LinetypeName) - Nom du type de ligne

## Classes Associées
- SelectionFilter, DxfCode

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
