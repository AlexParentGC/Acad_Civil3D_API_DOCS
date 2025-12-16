# Classe Preferences

## Vue d'Ensemble
La classe `Preferences` fournit un accès aux préférences et paramètres de l'application AutoCAD, similaire à la boîte de dialogue Options. Elle contient plusieurs sous-objets pour différentes catégories de paramètres.

## Namespace
`Autodesk.AutoCAD.ApplicationServices` (accédé via COM)

## Hiérarchie d'Héritage
```
System.Object
  └─ AcadPreferences (objet COM)
```

## Sous-Objets Clés

| Sous-Objet | Description |
|------------|-------------|
| `Files` | Chemins de fichiers et emplacements |
| `Display` | Paramètres d'affichage |
| `OpenSave` | Paramètres d'ouverture et de sauvegarde |
| `Output` | Paramètres de tracé et de publication |
| `System` | Paramètres système |
| `User` | Préférences utilisateur |
| `Drafting` | Paramètres de dessin |
| `Selection` | Paramètres de sélection |
| `Profiles` | Gestion de profil |

## Exemples de Code

### Exemple 1: Accéder aux Préférences de Fichiers
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic filesPrefs = prefs.Files;

// Obtenir les chemins de support
string supportPath = filesPrefs.SupportPath;
ed.WriteMessage($"\nChemin de Support : {supportPath}");

// Obtenir le chemin du modèle
string templatePath = filesPrefs.QNewTemplateFile;
ed.WriteMessage($"\nModèle par Défaut : {templatePath}");

// Obtenir les chemins de fichiers de dessin
string drawingPath = filesPrefs.DefaultInternetURL;
ed.WriteMessage($"\nChemin de Dessin par Défaut : {drawingPath}");
```

### Exemple 2: Modifier les Chemins de Fichiers
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic filesPrefs = prefs.Files;

// Ajouter au chemin de support
string currentPath = filesPrefs.SupportPath;
string newPath = "C:\\MonCheminCustom";

if (!currentPath.Contains(newPath))
{
    filesPrefs.SupportPath = currentPath + ";" + newPath;
    ed.WriteMessage($"\nAjouté {newPath} au chemin de support");
}
```

### Exemple 3: Préférences d'Affichage
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic displayPrefs = prefs.Display;

// Obtenir les paramètres d'affichage
bool showScrollBars = displayPrefs.DisplayScrollBars;
int crosshairSize = displayPrefs.CursorSize;

ed.WriteMessage($"\nAfficher Barres de Défilement : {showScrollBars}");
ed.WriteMessage($"\nTaille Réticule : {crosshairSize}%");

// Modifier les paramètres d'affichage
displayPrefs.CursorSize = 50; // Définir à 50%
```

### Exemple 4: Préférences Système
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic systemPrefs = prefs.System;

// Obtenir les paramètres système
bool singleDocMode = systemPrefs.SingleDocumentMode;
bool beepOnError = systemPrefs.BeepOnError;

ed.WriteMessage($"\nMode Document Unique : {singleDocMode}");
ed.WriteMessage($"\nBip sur Erreur : {beepOnError}");
```

### Exemple 5: Préférences Ouverture/Sauvegarde
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic openSavePrefs = prefs.OpenSave;

// Obtenir les paramètres de sauvegarde
int saveInterval = openSavePrefs.AutoSaveInterval;
string autoSavePath = openSavePrefs.AutoSavePath;

ed.WriteMessage($"\nIntervalle Auto-sauvegarde : {saveInterval} minutes");
ed.WriteMessage($"\nChemin Auto-sauvegarde : {autoSavePath}");

// Modifier les paramètres de sauvegarde
openSavePrefs.AutoSaveInterval = 10; // Définir à 10 minutes
```

### Exemple 6: Préférences Utilisateur
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic userPrefs = prefs.User;

// Obtenir les paramètres utilisateur
int undoLevels = userPrefs.UndoLevels;
bool rightClickCustomization = userPrefs.SCMDefaultMode;

ed.WriteMessage($"\nNiveaux Annulation : {undoLevels}");

// Modifier les paramètres utilisateur
userPrefs.UndoLevels = 100; // Définir les niveaux d'annulation
```

## Propriétés Communes Préférences Fichiers

| Propriété | Description |
|-----------|-------------|
| `SupportPath` | Chemin de recherche de fichier de support |
| `QNewTemplateFile` | Modèle par défaut pour QNEW |
| `DefaultInternetURL` | Emplacement de dessin par défaut |
| `AutoSavePath` | Emplacement de fichier auto-sauvegarde |
| `TempFilePath` | Emplacement de fichiers temporaires |
| `LogFilePath` | Emplacement de fichier journal |
| `PlotLogFilePath` | Emplacement de fichier journal de tracé |

## Propriétés Communes Préférences Affichage

| Propriété | Description |
|-----------|-------------|
| `DisplayScrollBars` | Afficher les barres de défilement |
| `CursorSize` | Taille du curseur réticule (1-100) |
| `LayoutDisplayMargins` | Afficher les marges dans les présentations |
| `LayoutShowPlotSetup` | Afficher la configuration de tracé dans les présentations |

## Propriétés Communes Préférences Système

| Propriété | Description |
|-----------|-------------|
| `SingleDocumentMode` | Mode SDI activé |
| `BeepOnError` | Bip sur erreur |
| `ShowWarningMessages` | Afficher les messages d'avertissement |

## Objets Associés
- [Application](../Application.md) - Fournit accès aux Préférences

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-AcadPreferences)
