# Converter Class

## Vue d'Ensemble
La classe `Converter` fournit des méthodes pour convertir entre différents types de données AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Méthodes Statiques

| Méthode | Description |
|---------|-------------|
| `DistanceToString(double, DistanceUnitFormat)` | Convertit une distance en chaîne |
| `AngleToString(double, AngleUnitFormat)` | Convertit un angle en chaîne |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

double distance = 100.5;
string distStr = Converter.DistanceToString(distance, DistanceUnitFormat.Decimal);
ed.WriteMessage($"\nDistance : {distStr}");
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
