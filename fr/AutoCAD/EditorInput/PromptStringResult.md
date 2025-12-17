# PromptStringResult Class

## Vue d'Ensemble
Résultat d'une opération d'invite de chaîne.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `StringResult` - Valeur de chaîne entrée

## Exemple de Code
```csharp
PromptStringOptions pso = new PromptStringOptions("\nEntrer le nom : ");
PromptResult pr = ed.GetString(pso);
if (pr.Status == PromptStatus.OK)
{
    string name = pr.StringResult;
}
```

## Classes Associées
- PromptStringOptions, PromptResult

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
