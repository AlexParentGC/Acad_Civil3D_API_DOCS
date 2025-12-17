# PromptResult Class

## Vue d'Ensemble
Classe de base pour tous les types de résultats d'invite.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Valeur PromptStatus
- `StringResult` - Résultat sous forme de chaîne (pour mots-clés/chaînes)

## Exemple de Code
```csharp
PromptResult pr = ed.GetString("\nEntrer le texte : ");
if (pr.Status == PromptStatus.OK)
{
    string text = pr.StringResult;
}
```

## Classes Associées
- Toutes les classes de résultats d'invite

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
