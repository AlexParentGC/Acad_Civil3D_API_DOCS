# PromptDistanceResult Class

## Vue d'Ensemble
Résultat d'une opération d'invite de distance.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `Value` - Valeur de distance (double)

## Exemple de Code
```csharp
PromptDistanceOptions pdo = new PromptDistanceOptions("\nEntrer la distance : ");
pdo.BasePoint = new Point3d(0, 0, 0);
pdo.UseBasePoint = true;
PromptDoubleResult pdr = ed.GetDistance(pdo);
if (pdr.Status == PromptStatus.OK)
{
    double distance = pdr.Value;
    ed.WriteMessage($"\nDistance : {distance}");
}
```

## Classes Associées
- PromptDistanceOptions, PromptDoubleResult

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
