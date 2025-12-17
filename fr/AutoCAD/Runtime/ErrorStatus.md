# ErrorStatus Enumeration

## Vue d'Ensemble
L'énumération `ErrorStatus` définit les codes d'erreur AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Valeurs Courantes

| Valeur | Description |
|--------|-------------|
| `OK` | Opération réussie |
| `eInvalidInput` | Entrée invalide |
| `eNoActiveTransactions` | Aucune transaction active |
| `eNotOpenForWrite` | Objet non ouvert en écriture |
| `eWasErased` | Objet effacé |
| `eOnLockedLayer` | Objet sur calque verrouillé |

## Exemple de Code

```csharp
using Autodesk.AutoCAD.Runtime;

try
{
    // Code qui peut lever une exception
}
catch (Autodesk.AutoCAD.Runtime.Exception ex)
{
    if (ex.ErrorStatus == ErrorStatus.eNoActiveTransactions)
    {
        ed.WriteMessage("\nAucune transaction active");
    }
}
```

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
