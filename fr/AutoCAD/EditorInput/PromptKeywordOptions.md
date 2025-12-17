# PromptKeywordOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur de sélectionner un mot-clé.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `Keywords` - Collection de mots-clés disponibles
- `AllowNone` - Autoriser une réponse nulle
- `AppendKeywordsToMessage` - Ajouter les mots-clés au message

## Exemple de Code
```csharp
PromptKeywordOptions pko = new PromptKeywordOptions("\nChoisir une option : ");
pko.Keywords.Add("Oui");
pko.Keywords.Add("Non");
pko.Keywords.Default = "Oui";
PromptResult pr = ed.GetKeywords(pko);
if (pr.Status == PromptStatus.OK)
{
    string keyword = pr.StringResult;
}
```

## Classes Associées
- PromptResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
