# PromptPointResult Class

## Vue d'Ensemble
Résultat d'une invite de sélection de point.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `Value` - Point sélectionné (Point3d)

## Exemple de Code
```csharp
PromptPointResult ppr = ed.GetPoint("\nSélectionner un point : ");
if (ppr.Status == PromptStatus.OK)
{
    Point3d pt = ppr.Value;
    ed.WriteMessage($"\nPoint : ({pt.X}, {pt.Y}, {pt.Z})");
}
```

## Classes Associées
- PromptPointOptions, Point3d

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
