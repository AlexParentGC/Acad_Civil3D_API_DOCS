# Classe MenuBar

## Vue d'Ensemble
La classe `MenuBar` fournit l'accès à la barre de menu d'AutoCAD pour la personnalisation et l'interaction. Notez qu'il s'agit d'un objet COM de la bibliothèque AutoCAD ActiveX Automation.

## Namespace
`Autodesk.AutoCAD.Windows` (Objet COM)

## Exemples de Code

### Exemple 1: Accéder à la Barre de Menu
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuBar = Application.MenuBar;

// Obtenir propriétés de la barre de menu
int menuCount = menuBar.Count;
ed.WriteMessage($"\nNombre de menus : {menuCount}");
```

### Exemple 2: Itérer à Travers les Menus
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuBar = Application.MenuBar;

ed.WriteMessage("\nÉléments de la Barre de Menu :");
for (int i = 0; i < menuBar.Count; i++)
{
    dynamic menu = menuBar.Item(i);
    ed.WriteMessage($"\n  {menu.Name}");
}
```

## Objets Associés
- [Application](Application.md) - Fournit l'accès à MenuBar
- [MenuGroups](MenuGroups.md) - Collection de groupes de menus

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
