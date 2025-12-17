# PromptDistanceOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur d'entrer une valeur de distance.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `BasePoint` - Point de base pour la mesure de distance
- `UseBasePoint` - Utiliser le point de base
- `DefaultValue` - Valeur de distance par défaut
- `AllowNegative` - Autoriser les valeurs négatives
- `AllowZero` - Autoriser zéro
- `UseDashedLine` - Afficher une ligne pointillée

## Exemple de Code
```csharp
PromptDistanceOptions pdo = new PromptDistanceOptions("\nEntrer la distance : ");
pdo.BasePoint = new Point3d(0, 0, 0);
pdo.UseBasePoint = true;
pdo.AllowNegative = false;
PromptDoubleResult pdr = ed.GetDistance(pdo);
if (pdr.Status == PromptStatus.OK)
{
    double distance = pdr.Value;
}
```

## Classes Associées
- PromptDoubleResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
