# Classe Application

## Vue d'Ensemble
La classe `Application` est l'objet racine pour accéder à l'application AutoCAD et ses services. Elle fournit l'accès aux documents, éléments d'interface utilisateur, préférences et fonctionnalités au niveau du système.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ Application (classe statique)
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `DocumentManager` | `DocumentManager` | Obtient le gestionnaire de documents |
| `MainWindow` | `MainWindow` | Obtient la fenêtre principale d'AutoCAD |
| `MenuBar` | `MenuBar` | Obtient la barre de menu |
| `MenuGroups` | `MenuGroups` | Obtient la collection des groupes de menus |
| `Preferences` | `PreferencesFiles` | Obtient les préférences de l'application |
| `Publisher` | `Publisher` | Obtient l'objet éditeur |
| `StatusBar` | `StatusBar` | Obtient la barre d'état |
| `UserConfigurationManager` | `UserConfigurationManager` | Obtient la configuration utilisateur |
| `InfoCenter` | `InfoCenter` | Obtient l'InfoCenter |
| `Version` | `string` | Obtient la version d'AutoCAD |
| `AcadApplication` | `object` | Obtient l'objet d'application COM |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `ShowAlertDialog(string)` | `void` | Affiche une boîte de dialogue d'alerte |
| `ShowModalDialog(Form)` | `DialogResult` | Affiche une boîte de dialogue modale |
| `ShowModelessDialog(Form)` | `void` | Affiche une boîte de dialogue non modale |
| `Idle` | `event` | Se déclenche lorsque l'application est inactive |

## Exemples de Code

### Exemple 1: Accéder à l'Application
```csharp
using Autodesk.AutoCAD.ApplicationServices;

// Obtenir le gestionnaire de documents
DocumentManager docMgr = Application.DocumentManager;

// Obtenir le document actif
Document acDoc = docMgr.MdiActiveDocument;

// Obtenir la version d'AutoCAD
string version = Application.Version;
ed.WriteMessage($"\nVersion AutoCAD : {version}");
```

### Exemple 2: Accéder à la Fenêtre Principale
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;

// Obtenir la fenêtre principale pour parent de dialogue
MainWindow mainWin = Application.MainWindow;

// Utiliser comme parent pour les dialogues WPF
System.Windows.Window myDialog = new System.Windows.Window();
myDialog.Owner = mainWin;
myDialog.ShowDialog();
```

### Exemple 3: Travailler avec la Barre d'État
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

// Définir le texte de la barre d'état
statusBar.SetMessageString("Traitement en cours...");

// Afficher la progression
for (int i = 0; i <= 100; i += 10)
{
    statusBar.SetProgressMeter($"Progression : {i}%");
    System.Threading.Thread.Sleep(100);
}

statusBar.SetMessageString("Terminé !");
```

### Exemple 4: Accéder aux Préférences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

PreferencesFiles prefs = Application.Preferences.Files;

// Obtenir les chemins de support
string supportPaths = prefs.SupportPath;
ed.WriteMessage($"\nChemins de support : {supportPaths}");

// Obtenir le chemin du modèle
string templatePath = prefs.QNewTemplateFile;
ed.WriteMessage($"\nModèle par défaut : {templatePath}");
```

### Exemple 5: Utiliser l'Événement Idle
```csharp
using Autodesk.AutoCAD.ApplicationServices;

public void RegisterIdleEvent()
{
    Application.Idle += OnIdle;
}

private void OnIdle(object sender, EventArgs e)
{
    // Code à exécuter quand AutoCAD est inactif
    // Attention - cela se déclenche fréquemment !
}
```

### Exemple 6: Afficher une Boîte d'Alerte
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Application.ShowAlertDialog("Ceci est un message d'alerte !");
```

### Exemple 7: Accéder à la Barre de Menu
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;

MenuBar menuBar = Application.MenuBar;

// Accéder aux groupes de menus
MenuGroups menuGroups = Application.MenuGroups;

foreach (MenuGroup mg in menuGroups)
{
    ed.WriteMessage($"\nGroupe de Menu : {mg.Name}");
}
```

## Sous-objets Application

### MainWindow
Représente la fenêtre principale de l'application AutoCAD. Utilisée comme parent pour les dialogues.

### MenuBar
Fournit l'accès à la barre de menu AutoCAD pour la personnalisation.

### MenuGroups
Collection de groupes de menus chargés dans AutoCAD.

### Preferences
Accès à toutes les préférences et paramètres AutoCAD :
- Préférences de fichiers
- Préférences d'affichage
- Préférences d'ouverture et de sauvegarde
- Préférences système
- Préférences utilisateur

### Publisher
Gère les opérations de tracé par lots et de publication.

### StatusBar
Contrôle l'affichage de la barre d'état et les indicateurs de progression.

### UserConfigurationManager
Gère les paramètres de configuration spécifiques à l'utilisateur.

### InfoCenter
Accès au système de recherche et d'aide InfoCenter.

## Objets Associés
- [DocumentManager](DocumentManager.md) - Gère les documents ouverts
- [Document](Document.md) - Document de dessin individuel
- [Database](Database.md) - Base de données du dessin
- [Editor](Editor.md) - Interaction utilisateur

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_Application)
