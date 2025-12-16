# Classe Profile

## Vue d'Ensemble
La classe `Profile` représente un profil en long vertical le long d'un axe dans Civil 3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Profile
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom du profil |
| `Description` | `string` | Obtient/définit la description |
| `AlignmentId` | `ObjectId` | Obtient l'ObjectId de l'axe parent |
| `StartingStation` | `double` | Obtient la station de départ |
| `EndingStation` | `double` | Obtient la station de fin |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `ElevationAt(double)` | `double` | Obtient l'élévation à la station |

## Exemples de Code

### Exemple 1: Obtenir l'Élévation du Profil
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    ObjectIdCollection profileIds = alignment.GetProfileIds();
    
    if (profileIds.Count > 0)
    {
        Profile profile = tr.GetObject(profileIds[0], OpenMode.ForRead) as Profile;
        
        double station = 100.0;
        double elevation = profile.ElevationAt(station);
        
        ed.WriteMessage($"\nProfil : {profile.Name}");
        ed.WriteMessage($"\nÉlévation à la Station {station} : {elevation:F2}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Alignment](Alignment.md) - Axe parent
- [CivilDocument](../Core/CivilDocument.md) - Conteneur de document

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Profile)
