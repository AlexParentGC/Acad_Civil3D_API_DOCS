# Classe Assembly

## Vue d'Ensemble
La classe `Assembly` représente un assemblage de projet 3D (corridor) dans Civil 3D, qui définit le modèle de section transversale (profil type) pour un projet 3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Assembly
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom de l'assemblage |
| `Description` | `string` | Obtient/définit la description |
| `CodeSetStyleId` | `ObjectId` | Obtient/définit le style de jeu de codes |

## Exemples de Code

### Exemple 1: Lister les Assemblages
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection assemblyIds = civilDoc.GetAssemblyIds();
    
    ed.WriteMessage($"\nTrouvé {assemblyIds.Count} assemblages :");
    
    foreach (ObjectId assemblyId in assemblyIds)
    {
        Assembly assembly = tr.GetObject(assemblyId, OpenMode.ForRead) as Assembly;
        
        ed.WriteMessage($"\n  {assembly.Name}");
        if (!string.IsNullOrEmpty(assembly.Description))
        {
            ed.WriteMessage($" - {assembly.Description}");
        }
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Corridor](Corridor.md) - Utilise les assemblages pour les sections transversales
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour assemblages

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Assembly)
