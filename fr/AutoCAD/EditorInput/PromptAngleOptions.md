# PromptAngleOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur d'entrer une valeur d'angle.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `BasePoint` - Point de base pour la mesure d'angle
- `UseBasePoint` - Utiliser le point de base
- `DefaultValue` - Valeur d'angle par défaut
- `UseAngleBase` - Utiliser le paramètre de base d'angle
- `UseDashedLine` - Afficher une ligne pointillée

## Exemple de Code
```csharp
PromptAngleOptions pao = new PromptAngleOptions("\nEntrer l'angle : ");
pao.BasePoint = new Point3d(0, 0, 0);
pao.UseBasePoint = true;
PromptDoubleResult pdr = ed.GetAngle(pao);
if (pdr.Status == PromptStatus.OK)
{
    double angle = pdr.Value;
}
```

## Classes Associées
- PromptDoubleResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
