# PromptEntityResult Class

## Vue d'Ensemble
Résultat d'une invite de sélection d'entité.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Status` - Statut de l'invite
- `ObjectId` - ObjectId de l'entité sélectionnée
- `PickedPoint` - Point où l'entité a été sélectionnée

## Exemple de Code
```csharp
PromptEntityResult per = ed.GetEntity("\nSélectionner une entité : ");
if (per.Status == PromptStatus.OK)
{
    ObjectId id = per.ObjectId;
    Point3d pickPt = per.PickedPoint;
}
```

## Classes Associées
- PromptEntityOptions, ObjectId

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
