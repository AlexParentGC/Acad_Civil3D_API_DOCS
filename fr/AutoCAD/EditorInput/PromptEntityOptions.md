# PromptEntityOptions Class

## Vue d'Ensemble
Définit les options pour demander à l'utilisateur de sélectionner une seule entité.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Propriétés Clés
- `Message` - Message d'invite
- `AllowNone` - Autoriser une réponse nulle
- `AllowObjectOnLockedLayer` - Autoriser les objets sur les calques verrouillés
- `Keywords` - Mots-clés disponibles

## Exemple de Code
```csharp
PromptEntityOptions peo = new PromptEntityOptions("\nSélectionner une entité : ");
peo.SetRejectMessage("\nSélection invalide");
peo.AddAllowedClass(typeof(Line), true);
PromptEntityResult per = ed.GetEntity(peo);
if (per.Status == PromptStatus.OK)
{
    ObjectId id = per.ObjectId;
}
```

## Classes Associées
- PromptEntityResult, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
