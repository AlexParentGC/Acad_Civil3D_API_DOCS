# PromptIntegerResult Class

## Vue d'Ensemble
Résultat d'une invite de valeur entière.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `Value` - Valeur entière entrée

## Exemple de Code
```csharp
PromptIntegerResult pir = ed.GetInteger("\nEntrer le compte : ");
if (pir.Status == PromptStatus.OK)
{
    int count = pir.Value;
    ed.WriteMessage($"\nCompte : {count}");
}
```

## Classes Associées
- PromptIntegerOptions, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
