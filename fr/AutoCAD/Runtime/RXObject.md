# RXObject Class

## Vue d'Ensemble
La classe `RXObject` est la classe de base pour tous les objets d'exécution AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Méthodes

| Méthode | Description |
|---------|-------------|
| `GetClass(Type)` | Obtient la classe RX pour un type |
| `Dispose()` | Libère les ressources |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

RXClass rxClass = RXObject.GetClass(typeof(Circle));
ed.WriteMessage($"\nClasse : {rxClass.Name}");
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
