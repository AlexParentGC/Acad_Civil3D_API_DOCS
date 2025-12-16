# Interface Ribbon (Ruban)

## Vue d'Ensemble
L'API Ribbon permet aux développeurs de personnaliser par programmation l'interface du Ruban AutoCAD (Onglets, Panneaux et Boutons). Cette fonctionnalité est fournie par l'assemblage `AdWindows.dll` (namespace `Autodesk.Windows`), qui est séparé des DLLs d'API AutoCAD standard.

> [!IMPORTANT]
> **AdWindows.dll Requis** : Pour utiliser ces fonctionnalités, vous devez ajouter une référence à `AdWindows.dll` trouvé dans le répertoire d'installation d'AutoCAD.

## Namespace
`Autodesk.Windows`

## Classes Clés

| Classe | Description |
|--------|-------------|
| `ComponentManager` | Gestionnaire statique pour accéder au `RibbonControl`. |
| `RibbonControl` | Représente l'interface utilisateur Ruban entière. |
| `RibbonTab` | Un onglet spécifique (ex : "Début", "Plug-ins"). |
| `RibbonPanel` | Un conteneur dans un onglet. |
| `RibbonButton` | Un élément bouton cliquable. |
| `RibbonPanelSource` | Contenu défini pour un panneau. |

## Exemples de Code

### Exemple 1: Créer un Onglet et un Panneau Personnalisés
```csharp
using Autodesk.Windows;
using System.Windows.Media.Imaging; // Pour les icônes

public void CreateRibbon()
{
    RibbonControl ribbon = ComponentManager.Ribbon;
    if (ribbon == null) return;

    // 1. Créer l'Onglet
    RibbonTab tab = new RibbonTab();
    tab.Title = "Mes Outils";
    tab.Id = "MES_OUTILS_TAB";
    
    // 2. Créer la Source du Panneau (Contenu)
    RibbonPanelSource panelSource = new RibbonPanelSource();
    panelSource.Title = "Général";
    
    // 3. Créer le Panneau (Conteneur visuel)
    RibbonPanel panel = new RibbonPanel();
    panel.Source = panelSource;
    
    // 4. Ajouter des éléments à la Source du Panneau
    RibbonButton btn = new RibbonButton();
    btn.Text = "Lancer Cmd";
    btn.ShowText = true;
    btn.CommandParameter = "MACOMMANDE "; // Notez l'espace
    btn.CommandHandler = new AdskCommandHandler(); // Gestionnaire personnalisé optionnel
    
    panelSource.Items.Add(btn);
    
    // 5. Assembler
    tab.Panels.Add(panel);
    ribbon.Tabs.Add(tab);
    
    // 6. Rendre Actif
    tab.IsActive = true;
}
```

### Exemple 2: Ajouter un Bouton à un Onglet Existant
```csharp
public void AddToAddinsTab()
{
    RibbonControl ribbon = ComponentManager.Ribbon;
    RibbonTab addinsTab = null;

    // Trouver l'onglet "Add-ins" (Compléments)
    foreach (RibbonTab tab in ribbon.Tabs)
    {
        if (tab.Id == "ACAD.ADDINS") // ID Standard
        {
            addinsTab = tab;
            break;
        }
    }

    if (addinsTab != null)
    {
        // Ajouter un panneau personnalisé à l'onglet existant
        // ... (logique de création de panneau) ...
        addinsTab.Panels.Add(myPanel);
    }
}
```

### Exemple 3: Définir un Gestionnaire de Commande
```csharp
// Pratique standard : Les boutons envoient des chaînes de commande à AutoCAD
// Mais vous pouvez aussi intercepter les clics directement en utilisant ICommand
public class AdskCommandHandler : System.Windows.Input.ICommand
{
    public bool CanExecute(object parameter) => true;
    public event EventHandler CanExecuteChanged;

    public void Execute(object parameter)
    {
        if (parameter is RibbonButton btn)
        {
            // Logique C# personnalisée ici
            System.Windows.MessageBox.Show("Cliqué " + btn.Text);
        }
    }
}
```

### Exemple 4: Ajouter des Icônes
```csharp
// Les Rubans utilisent des ImageSources WPF
btn.Image = LoadImage("my_icon_16.png");
btn.LargeImage = LoadImage("my_icon_32.png");
btn.Size = RibbonItemSize.Large;

// Helper pour charger la ressource
private System.Windows.Media.ImageSource LoadImage(string resourceName)
{
    // ... Logique de chargement BitmapImage WPF ...
    return new BitmapImage(new Uri("pack://application:,,,/MyAssembly;component/" + resourceName));
}
```

### Exemple 5: Créer des Boutons Divisés (Split Buttons)
```csharp
RibbonSplitButton splitBtn = new RibbonSplitButton();
splitBtn.Text = "Options";
splitBtn.ShowText = true;

splitBtn.Items.Add(new RibbonButton { Text = "Option A" });
splitBtn.Items.Add(new RibbonButton { Text = "Option B" });
```

### Exemple 6: Gérer l'État du Ruban
```csharp
// Vérifier si le Ruban existe (il peut être fermé par l'utilisateur)
if (ComponentManager.Ribbon == null) return;

// Forcer la reconstruction/mise à jour si les changements dynamiques n'apparaissent pas
// généralement non nécessaire si ajouté aux collections correctement.
```

### Exemple 7: Info-bulles (Tooltips)
```csharp
btn.ToolTip = "Ceci est une info-bulle basique";
// Contenu amélioré
RibbonToolTip extendedTip = new RibbonToolTip();
extendedTip.Title = "Aide Détillée";
extendedTip.Content = "Cette commande effectue un calcul complexe.";
extendedTip.ExpandedContent = "Plus de détails...";
// btn.ToolTip = extendedTip; // Assigner objet
```

### Exemple 8: Créer des Panneaux de Rangée (Flux)
```csharp
RibbonRowPanel row = new RibbonRowPanel(); 
// Ajoute les éléments horizontalement dans le panneau
row.Items.Add(btn1);
row.Items.Add(btnSeparator);
row.Items.Add(btn2);
panelSource.Items.Add(row);
```

## Meilleures Pratiques
1. **CUIx vs API** : Il est généralement plus facile et plus robuste de définir des Rubans utilisant un fichier `.cuix` (Custom User Interface) plutôt que de coder en dur la génération C#. L'API est meilleure pour les modifications *dynamiques* lors de l'exécution.
2. **Références** : Vous devez référencer `AdWindows.dll`, `PresentationCore`, `PresentationFramework`, et `WindowsBase`.
3. **Identifiants** : Utilisez des IDs uniques pour les onglets et panneaux pour éviter les conflits avec d'autres plugins.
4. **Icônes** : Utilisez des ressources intégrées pour les icônes pour garder le déploiement simple.

## Objets Associés
- [PaletteSet](PaletteSet.md)
- [Application](../Core/Application.md)

## Références
- [API AdWindows (Docs non officielles existent, l'officiel est rare)](https://help.autodesk.com/view/OARX/2024/ENU/)
