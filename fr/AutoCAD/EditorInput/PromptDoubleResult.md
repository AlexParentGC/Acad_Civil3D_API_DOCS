# PromptDoubleResult Class

## Vue d'Ensemble
Résultat d'une invite de valeur double.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `Value` - Valeur double entrée

## Exemple de Code
```csharp
PromptDoubleResult pdr = ed.GetDouble("\nEntrer la valeur : ");
if (pdr.Status == PromptStatus.OK)
{
    double val = pdr.Value;
    ed.WriteMessage($"\nValeur : {val}");
}
```

## Classes Associées
- PromptDoubleOptions, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
