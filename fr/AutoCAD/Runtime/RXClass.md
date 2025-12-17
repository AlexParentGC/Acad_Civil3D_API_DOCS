# RXClass Class

## Vue d'Ensemble
La classe `RXClass` représente une classe d'exécution AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Propriétés

| Propriété | Description |
|-----------|-------------|
| `Name` | Nom de la classe |
| `DxfName` | Nom DXF de la classe |
| `AppName` | Nom de l'application |
| `MyParent` | Classe parente |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

RXClass rxClass = RXObject.GetClass(typeof(Line));
ed.WriteMessage($"\nNom de classe : {rxClass.Name}");
ed.WriteMessage($"\nNom DXF : {rxClass.DxfName}");
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
