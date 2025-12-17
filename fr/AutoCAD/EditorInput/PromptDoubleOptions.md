# PromptDoubleOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur d'entrer une valeur double.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `DefaultValue` - Valeur par défaut
- `AllowNegative` - Autoriser les valeurs négatives
- `AllowZero` - Autoriser zéro
- `AllowNone` - Autoriser une réponse nulle
- `UseDefaultValue` - Utiliser la valeur par défaut

## Exemple de Code
```csharp
PromptDoubleOptions pdo = new PromptDoubleOptions("\nEntrer la valeur : ");
pdo.AllowNegative = false;
pdo.DefaultValue = 10.0;
PromptDoubleResult pdr = ed.GetDouble(pdo);
if (pdr.Status == PromptStatus.OK)
{
    double value = pdr.Value;
}
```

## Classes Associées
- PromptDoubleResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
