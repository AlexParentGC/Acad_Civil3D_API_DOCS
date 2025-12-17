# Exception Class

## Vue d'Ensemble
La classe `Exception` représente une exception AutoCAD avec un code d'erreur.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Propriétés

| Propriété | Description |
|-----------|-------------|
| `ErrorStatus` | Code d'erreur ErrorStatus |
| `Message` | Message d'erreur |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

try
{
    // Code AutoCAD
}
catch (Autodesk.AutoCAD.Runtime.Exception ex)
{
    ed.WriteMessage($"\nErreur AutoCAD : {ex.Message}");
    ed.WriteMessage($"\nCode d'erreur : {ex.ErrorStatus}");
}
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
