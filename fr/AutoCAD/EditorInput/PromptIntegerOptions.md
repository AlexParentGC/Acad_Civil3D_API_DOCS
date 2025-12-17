# PromptIntegerOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur d'entrer une valeur entière.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `DefaultValue` - Valeur entière par défaut
- `AllowNegative` - Autoriser les valeurs négatives
- `AllowZero` - Autoriser zéro
- `AllowNone` - Autoriser une réponse nulle

## Exemple de Code
```csharp
PromptIntegerOptions pio = new PromptIntegerOptions("\nEntrer le compte : ");
pio.AllowNegative = false;
pio.DefaultValue = 1;
PromptIntegerResult pir = ed.GetInteger(pio);
if (pir.Status == PromptStatus.OK)
{
    int count = pir.Value;
}
```

## Classes Associées
- PromptIntegerResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
