# Classe MenuGroups

## Vue d'Ensemble
La classe `MenuGroups` représente la collection des groupes de menus chargés dans AutoCAD. Les groupes de menus sont associés aux fichiers CUIx et contiennent des menus, des barres d'outils, et d'autres éléments d'interface utilisateur.

## Namespace
`Autodesk.AutoCAD.Windows` (Objet COM)

## Exemples de Code

### Exemple 1: Lister les Groupes de Menus
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuGroups = Application.MenuGroups;

ed.WriteMessage($"\nTotal Groupes de Menus : {menuGroups.Count}");
ed.WriteMessage("\nGroupes de Menus :");

for (int i = 0; i < menuGroups.Count; i++)
{
    dynamic menuGroup = menuGroups.Item(i);
    ed.WriteMessage($"\n  {menuGroup.Name}");
}
```

### Exemple 2: Trouver un Groupe de Menu Spécifique
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuGroups = Application.MenuGroups;

string targetGroup = "ACAD";

for (int i = 0; i < menuGroups.Count; i++)
{
    dynamic menuGroup = menuGroups.Item(i);
    
    if (menuGroup.Name == targetGroup)
    {
        ed.WriteMessage($"\nTrouvé groupe de menu : {menuGroup.Name}");
        
        // Accéder aux menus dans ce groupe
        dynamic menus = menuGroup.Menus;
        ed.WriteMessage($"\n  Menus dans le groupe : {menus.Count}");
        break;
    }
}
```

### Exemple 3: Charger un Groupe de Menu
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuGroups = Application.MenuGroups;

// Charger un fichier CUIx
string cuixPath = "C:\\MyCustom\\MyMenu.cuix";

try
{
    dynamic newGroup = menuGroups.Load(cuixPath);
    ed.WriteMessage($"\nGroupe de menu chargé : {newGroup.Name}");
}
catch (System.Exception ex)
{
    ed.WriteMessage($"\nErreur lors du chargement du groupe de menu : {ex.Message}");
}
```

## Objets Associés
- [Application](Application.md) - Fournit l'accès à MenuGroups
- [MenuBar](MenuBar.md) - Barre de menu

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
