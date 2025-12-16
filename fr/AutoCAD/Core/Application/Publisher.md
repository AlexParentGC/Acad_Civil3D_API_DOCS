# Classe Publisher

## Vue d'Ensemble
La classe `Publisher` gère les opérations de tracé par lots et de publication dans AutoCAD, vous permettant de publier plusieurs feuilles vers DWF, PDF ou un traceur.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Exemples de Code

### Exemple 1: Accéder au Publisher
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic publisher = Application.Publisher;

// Le Publisher est disponible pour les opérations de tracé par lots
ed.WriteMessage("\nObjet Publisher accédé");
```

### Exemple 2: Publier vers PDF (Conceptuel)
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.PlottingServices;

// Note : La publication nécessite typiquement PlotEngine et configuration de fichier DSD
// Ceci est un exemple conceptuel simplifié

dynamic publisher = Application.Publisher;

// Créer le fichier DSD (Drawing Set Descriptions)
// Configurer les feuilles à publier
// Exécuter l'opération de publication

ed.WriteMessage("\nOpération de publication initiée");
```

## Objets Associés
- [Application](../Application.md) - Fournit accès au Publisher
- PlotEngine - Moteur de tracé
- DsdData - Données de description de jeu de dessins

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
