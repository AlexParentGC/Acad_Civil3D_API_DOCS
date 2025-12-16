# Classe ContextMenu

## Vue d'Ensemble
AutoCAD permet aux développeurs d'ajouter des éléments de menu personnalisés au menu contextuel (clic droit) de types d'entités spécifiques ou au menu contextuel par défaut. Cela se fait via la classe `ContextMenuExtension`.

## Namespace
`Autodesk.AutoCAD.Windows`

## Classes Clés

| Classe | Description |
|--------|-------------|
| `ContextMenuExtension` | Une définition de menu personnalisée. |
| `MenuItem` | Un élément individuel cliquable. |
| `Application.AddObjectContextMenuExtension` | Enregistre le menu pour un type d'entité. |
| `Application.AddDefaultContextMenuExtension` | Enregistre le menu pour l'état "rien de sélectionné". |

## Exemples de Code

### Exemple 1: Ajouter un Menu à un Type d'Entité Spécifique
```csharp
using Autodesk.AutoCAD.Windows;
using Autodesk.AutoCAD.Runtime;

public class MyPlugin : IExtensionApplication
{
    private ContextMenuExtension _circleMenu;

    public void Initialize()
    {
        _circleMenu = new ContextMenuExtension();
        _circleMenu.Title = "Outils Cercle"; // Titre du sous-menu (optionnel)
        
        MenuItem item = new MenuItem("Calculer Info Aire");
        item.Click += OnCalculateArea;
        _circleMenu.MenuItems.Add(item);
        
        // Enregistrer pour TOUS les objets Cercle
        Application.AddObjectContextMenuExtension(
            RXObject.GetClass(typeof(Circle)), 
            _circleMenu
        );
    }

    public void Terminate()
    {
        // Toujours nettoyer
        Application.RemoveObjectContextMenuExtension(
             RXObject.GetClass(typeof(Circle)), 
             _circleMenu
        );
    }

    private void OnCalculateArea(object sender, EventArgs e)
    {
        // Logique contextuelle
        // Les objets sélectionnés sont dans le SelectionSet
        // ...
    }
}
```

### Exemple 2: Ajouter au Menu Contextuel Global
```csharp
// Ce menu apparaît lorsque vous faites un clic droit dans l'espace vide
Application.AddDefaultContextMenuExtension(_myMenu);
```

### Exemple 3: Gérer l'Événement Clic
```csharp
private void OnMenuClick(object sender, EventArgs e)
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    
    // Exécuter la logique
    // Note : Le menu contextuel s'exécute dans le thread UI.
    // Si vous devez exécuter une commande, utilisez SendStringToExecute
    doc.SendStringToExecute("MA_COMMANDE ", true, false, false);
}
```

### Exemple 4: Activation/Désactivation Dynamique
```csharp
_menu.Popup += (s, e) =>
{
    MenuItem item = _menu.MenuItems[0];
    
    // Vérifier condition
    bool valid = CheckSomeCondition();
    
    item.Enabled = valid;
    item.Visible = true; // ou le cacher
};
```

### Exemple 5: Sous-menus
```csharp
MenuItem parentItem = new MenuItem("Outils Avancés");

MenuItem child1 = new MenuItem("Outil A");
MenuItem child2 = new MenuItem("Outil B");

parentItem.MenuItems.Add(child1);
parentItem.MenuItems.Add(child2);

_menu.MenuItems.Add(parentItem);
```

### Exemple 6: Séparateurs
```csharp
MenuItem separator = new MenuItem(""); 
// Assigner un style de séparateur standard pourrait être requis selon la version de l'API
// Ou simplement ajouter un séparateur directement :
_menu.MenuItems.Add(new MenuItem(null)); // Souvent interprété comme séparateur
```

### Exemple 7: Accéder à l'Objet Sélectionné
```csharp
// Lors d'un clic droit sur un objet, le SelectionSet valide le contient généralement
private void OnEntityClick(object sender, EventArgs e)
{
    Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
    PromptSelectionResult sel = ed.SelectImplied(); // La logique varie
    
    if (sel.Status == PromptStatus.OK)
    {
        // Traiter objet sélectionné
    }
}
```

### Exemple 8: Supprimer le Menu à l'Exécution
```csharp
// Vous pouvez supprimer des menus dynamiquement si les paramètres changent
Application.RemoveObjectContextMenuExtension(...);
```

## Meilleures Pratiques
1. **Nettoyer** : Supprimez toujours vos extensions dans la méthode `Terminate()` de votre `IExtensionApplication`. Ne pas le faire peut laisser des éléments de menu morts jusqu'au redémarrage d'AutoCAD.
2. **Performance** : N'effectuez pas de logique lourde dans la vérification de l'événement `Popup` ; cela retarde l'apparition du menu.
3. **Contexte** : Rappelez-vous que l'utilisateur peut avoir plusieurs objets sélectionnés. Votre commande doit gérer `ActiveSelectionSet`.

## Objets Associés
- [Application](../Core/Application.md)
- [Entity](../BaseClasses/Entity.md)

## Références
- [Référence ContextMenuExtension Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows_ContextMenuExtension)
