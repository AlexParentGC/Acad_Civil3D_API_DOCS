# Classe InfoCenter

## Vue d'Ensemble
La classe `InfoCenter` fournit un accès à l'InfoCenter d'AutoCAD, qui est le système de recherche et d'aide dans la barre de titre de l'application.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Exemples de Code

### Exemple 1: Accéder à InfoCenter
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic infoCenter = Application.InfoCenter;

// Accéder à InfoCenter pour la fonctionnalité de recherche et d'aide
ed.WriteMessage("\nInfoCenter accédé");
```

### Exemple 2: Intégration InfoCenter (Conceptuel)
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic infoCenter = Application.InfoCenter;

// InfoCenter peut être utilisé pour intégrer une aide personnalisée
// et une fonctionnalité de recherche dans le système d'aide d'AutoCAD

ed.WriteMessage("\nIntégration InfoCenter disponible");
```

## Objets Associés
- [Application](Application.md) - Fournit l'accès à InfoCenter

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
