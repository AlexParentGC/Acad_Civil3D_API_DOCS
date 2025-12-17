# AcRx Class

## Vue d'Ensemble
La classe `AcRx` fournit des méthodes utilitaires pour l'environnement d'exécution AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Méthodes Statiques

| Méthode | Description |
|---------|-------------|
| `GetClass(Type)` | Obtient la classe RX pour un type |
| `GetServiceClass(string)` | Obtient une classe de service par nom |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

RXClass rxClass = RXObject.GetClass(typeof(Line));
ed.WriteMessage($"\nNom de classe : {rxClass.Name}");
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
