# PromptStringOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur d'entrer une valeur de chaîne.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `DefaultValue` - Valeur de chaîne par défaut
- `AllowSpaces` - Autoriser les espaces dans l'entrée
- `UseDefaultValue` - Utiliser la valeur par défaut

## Exemple de Code
```csharp
PromptStringOptions pso = new PromptStringOptions("\nEntrer le nom : ");
pso.AllowSpaces = true;
pso.DefaultValue = "Défaut";
PromptResult pr = ed.GetString(pso);
if (pr.Status == PromptStatus.OK)
{
    string name = pr.StringResult;
}
```

## Classes Associées
- PromptResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
