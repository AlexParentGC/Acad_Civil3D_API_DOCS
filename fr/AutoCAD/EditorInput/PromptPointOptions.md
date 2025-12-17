# PromptPointOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur de sélectionner un point.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `BasePoint` - Point de base
- `UseBasePoint` - Utiliser le point de base
- `UseDashedLine` - Afficher une ligne pointillée
- `AllowNone` - Autoriser une réponse nulle

## Exemple de Code
```csharp
PromptPointOptions ppo = new PromptPointOptions("\nSélectionner un point : ");
ppo.BasePoint = new Point3d(0, 0, 0);
ppo.UseBasePoint = true;
PromptPointResult ppr = ed.GetPoint(ppo);
if (ppr.Status == PromptStatus.OK)
{
    Point3d pt = ppr.Value;
}
```

## Classes Associées
- PromptPointResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
