# LispFunctionAttribute Class

## Vue d'Ensemble
La classe `LispFunctionAttribute` est utilisée pour exposer des méthodes .NET à LISP.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

[LispFunction("MyLispFunction")]
public object MyLispFunction(ResultBuffer args)
{
    return "Résultat de la fonction LISP";
}
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
