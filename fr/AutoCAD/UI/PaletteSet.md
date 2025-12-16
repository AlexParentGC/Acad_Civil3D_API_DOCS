# Classe PaletteSet

## Vue d'Ensemble
`PaletteSet` représente une fenêtre flottante ou ancrée dans AutoCAD qui peut contenir plusieurs onglets (Palettes). C'est le mécanisme standard pour créer des outils d'interface utilisateur interactifs et non modaux (modeless), comme la Palette des Propriétés, le Gestionnaire de Calques, ou des interfaces d'outils personnalisées. Elle prend en charge l'hébergement de contrôles .NET standard `UserControl` (WinForms) ou `ElementHost` (WPF).

## Namespace
`Autodesk.AutoCAD.Windows`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Le titre du jeu de palettes. |
| `Visible` | `bool` | Contrôle la visibilité. |
| `Style` | `PaletteSetStyles` | Contrôle le comportement d'ancrage, de transparence et d'accrochage. |
| `Dock` | `DockSides` | Obtient ou définit la position d'ancrage actuelle. |
| `Count` | `int` | Nombre d'onglets (Palettes) dans le jeu. |
| `Opacity` | `int` | Niveau de transparence (0-100). |
| `KeepFocus` | `bool` | Si vrai, la palette garde le focus après interaction. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Add(string, Control)` | `Palette` | Ajoute un onglet avec un contrôle WinForms. |
| `AddVisual(string, Visual)` | `Palette` | Ajoute un visuel WPF (nécessite adaptation API). |
| `Close()` | `void` | Cache la palette (ne la détruit pas). |

## Exemples de Code

### Exemple 1: Créer un PaletteSet Basique
```csharp
using Autodesk.AutoCAD.Windows;
using System.Windows.Forms;

public class MyPlugin
{
    // Définir comme statique pour persister à travers les commandes
    private static PaletteSet _ps = null;

    [CommandMethod("ShowPalette")]
    public void ShowPalette()
    {
        if (_ps == null)
        {
            _ps = new PaletteSet("Mes Outils");
            _ps.Style = PaletteSetStyles.ShowAutoHideButton | 
                        PaletteSetStyles.ShowCloseButton |
                        PaletteSetStyles.Snappable;
            
            // Ajouter un UserControl simple
            UserControl myControl = new UserControl();
            myControl.Controls.Add(new Button { Text = "Cliquez-moi", Dock = DockStyle.Top });
            
            _ps.Add("Onglet 1", myControl);
        }
        
        _ps.Visible = true;
    }
}
```

### Exemple 2: Héberger du Contenu WPF
```csharp
// Contrôle Hôte Standard pour WPF
public class WpfHostControl : System.Windows.Forms.UserControl
{
    private System.Windows.Forms.Integration.ElementHost _host;
    
    public WpfHostControl(System.Windows.Controls.UserControl wpfControl)
    {
        _host = new System.Windows.Forms.Integration.ElementHost();
        _host.Dock = DockStyle.Fill;
        _host.Child = wpfControl;
        this.Controls.Add(_host);
    }
}

// Utilisation dans la Commande
_ps.Add("Onglet WPF", new WpfHostControl(new MyWpfUserControl()));
```

### Exemple 3: Gérer l'Ancrage
```csharp
// Forcer l'ancrage à gauche
_ps.Dock = DockSides.Left;

// Restreindre les options d'ancrage (ex : empêcher flottant)
_ps.DockEnabled = DockSides.Left | DockSides.Right;
```

### Exemple 4: Gérer les Événements
```csharp
_ps.StateChanged += (s, e) => 
{
    if (e.NewState == StateEventIndex.Hide)
    {
         // La palette a été cachée/réduite
    }
};

_ps.PaletteActivated += (s, e) =>
{
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nBasculé vers {e.Activated.Name}");
};
```

### Exemple 5: Sauvegarder l'État (Taille/Position)
```csharp
// Par défaut, AutoCAD sauvegarde l'état du PaletteSet dans le registre/profil basé sur le GUID.
// Pour assurer une sauvegarde unique, créez le PaletteSet avec un GUID spécifique.
_ps = new PaletteSet("Mes Outils", new Guid("D3D8C3E0-5F9B-4B52-8E3A-1C2D3E4F5A6B"));
```

### Exemple 6: Supprimer des Onglets
```csharp
if (_ps.Count > 0)
{
    // Supprimer l'onglet à l'index 0
    _ps.Remove(0);
}
```

### Exemple 7: Interaction Non Modale (Modeless)
```csharp
// Les palettes sont non modales. Elles peuvent interagir avec le dessin tant qu'elles sont valides.
// IMPORTANT : Vous devez verrouiller le document lors de la modification de la BD depuis un événement de Palette.
private void ButtonClick(object sender, EventArgs e)
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    
    // Logique de verrouillage de document
    using (doc.LockDocument())
    {
        using (Transaction tr = doc.Database.TransactionManager.StartTransaction())
        {
            // Modifier la base de données...
            tr.Commit();
        }
    }
}
```

### Exemple 8: Transparence
```csharp
// Définir opacité à 90%
_ps.Opacity = 90;
```

## Meilleures Pratiques
1. **Cycle de Vie Statique** : Gardez toujours une référence `static` à votre `PaletteSet`. Si vous en créez une nouvelle à chaque commande, vous aurez des fenêtres dupliquées.
2. **GUID** : Fournissez toujours un `Guid` générique dans le constructeur pour vous assurer qu'AutoCAD se souvienne de la position et de la taille de la fenêtre entre les sessions.
3. **Verrouillage de Document** : Les interactions utilisateur dans la palette s'exécutent dans le contexte du thread UI, pas le contexte de commande. Vous **DEVEZ** utiliser `doc.LockDocument()` avant de démarrer une transaction.
4. **WPF** : Préférez WPF pour une interface utilisateur moderne. Utilisez `ElementHost` pour faire le pont, car `PaletteSet` attend nativement des contrôles WinForms.

## Objets Associés
- [Editor](../Core/Editor.md) - Pour le retour d'information.
- [Application](../Core/Application.md) - Pour l'accès aux documents.

## Références
- [Référence PaletteSet Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows_PaletteSet)
