# Classe MainWindow

## Vue d'Ensemble
La classe `MainWindow` représente la fenêtre principale de l'application AutoCAD. Elle est principalement utilisée comme fenêtre parente pour les boîtes de dialogue afin de s'assurer qu'elles apparaissent correctement dans l'interface AutoCAD.

## Namespace
`Autodesk.AutoCAD.Windows`

## Hiérarchie d'Héritage
```
System.Object
  └─ System.Windows.Window
      └─ MainWindow
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Title` | `string` | Obtient/définit le titre de la fenêtre |
| `Width` | `double` | Obtient/définit la largeur de la fenêtre |
| `Height` | `double` | Obtient/définit la hauteur de la fenêtre |
| `Left` | `double` | Obtient/définit la position gauche |
| `Top` | `double` | Obtient/définit la position haute |

## Exemples de Code

### Exemple 1: Utiliser MainWindow comme Parent de Dialogue
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;
using System.Windows;

// Obtenir fenêtre principale
MainWindow mainWin = Application.MainWindow;

// Créer dialogue WPF
Window myDialog = new Window();
myDialog.Title = "Mon Dialogue";
myDialog.Width = 400;
myDialog.Height = 300;
myDialog.Owner = mainWin; // Définir AutoCAD comme parent
myDialog.WindowStartupLocation = WindowStartupLocation.CenterOwner;

// Afficher dialogue
myDialog.ShowDialog();
```

### Exemple 2: Utiliser avec Windows Forms
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;
using System.Windows.Forms;
using System.Windows.Interop;

MainWindow mainWin = Application.MainWindow;

// Créer dialogue Windows Forms
Form myForm = new Form();
myForm.Text = "Mon Formulaire";
myForm.Width = 400;
myForm.Height = 300;

// Définir AutoCAD comme parent utilisant le handle
WindowInteropHelper helper = new WindowInteropHelper(mainWin);
IWin32Window owner = new Win32Window(helper.Handle);
myForm.ShowDialog(owner);
```

### Exemple 3: Obtenir les Informations de la Fenêtre
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;

MainWindow mainWin = Application.MainWindow;

ed.WriteMessage($"\nTitre Fenêtre AutoCAD : {mainWin.Title}");
ed.WriteMessage($"\nTaille Fenêtre : {mainWin.Width} x {mainWin.Height}");
ed.WriteMessage($"\nPosition Fenêtre : ({mainWin.Left}, {mainWin.Top})");
```

## Classe Helper pour Windows Forms

```csharp
public class Win32Window : IWin32Window
{
    private IntPtr _handle;
    
    public Win32Window(IntPtr handle)
    {
        _handle = handle;
    }
    
    public IntPtr Handle
    {
        get { return _handle; }
    }
}
```

## Objets Associés
- [Application](Application.md) - Fournit l'accès à MainWindow
- [Document](Document.md) - Fenêtre de document

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows_MainWindow)
