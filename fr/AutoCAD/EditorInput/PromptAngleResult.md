# PromptAngleResult Class

## Vue d'Ensemble
Résultat d'une opération d'invite d'angle.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `Value` - Valeur d'angle en radians (double)

## Exemple de Code
```csharp
PromptAngleOptions pao = new PromptAngleOptions("\nEntrer l'angle : ");
PromptDoubleResult pdr = ed.GetAngle(pao);
if (pdr.Status == PromptStatus.OK)
{
    double angle = pdr.Value;
    double degrees = angle * 180 / Math.PI;
    ed.WriteMessage($"\nAngle : {degrees}°");
}
```

## Classes Associées
- PromptAngleOptions, PromptDoubleResult

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
