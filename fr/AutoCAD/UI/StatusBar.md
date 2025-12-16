# Classe StatusBar

## Vue d'Ensemble
La classe `StatusBar` fournit l'accès à la barre d'état d'AutoCAD pour afficher des messages et des indicateurs de progression.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ StatusBar
```

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SetMessageString(string)` | `void` | Définit la chaîne de message de la barre d'état |
| `SetProgressMeter(string)` | `void` | Définit le texte de la barre de progression |
| `SetProgressMeterLimit(int)` | `void` | Définit la valeur maximale de la barre de progression |
| `SetProgressMeterPos(int)` | `void` | Définit la position de la barre de progression |
| `ShowProgressMeter()` | `void` | Affiche la barre de progression |
| `HideProgressMeter()` | `void` | Cache la barre de progression |

## Exemples de Code

### Exemple 1: Message d'État Simple
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

statusBar.SetMessageString("Traitement des données...");

// Faire le travail
System.Threading.Thread.Sleep(2000);

statusBar.SetMessageString("Terminé !");
```

### Exemple 2: Barre de Progression
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

// Configurer barre de progression
statusBar.SetProgressMeterLimit(100);
statusBar.ShowProgressMeter();

for (int i = 0; i <= 100; i++)
{
    statusBar.SetProgressMeterPos(i);
    statusBar.SetMessageString($"Traitement : {i}%");
    
    // Faire le travail
    System.Threading.Thread.Sleep(50);
}

statusBar.HideProgressMeter();
statusBar.SetMessageString("Traitement terminé !");
```

### Exemple 3: Progression avec Plage Personnalisée
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

int totalItems = 250;
statusBar.SetProgressMeterLimit(totalItems);
statusBar.ShowProgressMeter();

for (int i = 0; i < totalItems; i++)
{
    statusBar.SetProgressMeterPos(i);
    statusBar.SetMessageString($"Traitement élément {i + 1} de {totalItems}");
    
    // Traiter élément
}

statusBar.HideProgressMeter();
statusBar.SetMessageString($"Traité {totalItems} éléments");
```

### Exemple 4: Progression avec Try-Finally
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

try
{
    statusBar.SetProgressMeterLimit(100);
    statusBar.ShowProgressMeter();
    
    for (int i = 0; i <= 100; i++)
    {
        statusBar.SetProgressMeterPos(i);
        
        // Faire travail qui pourrait lancer exception
        
        if (i == 50)
        {
            // Simuler erreur
            throw new System.Exception("Erreur à 50%");
        }
    }
}
catch (System.Exception ex)
{
    statusBar.SetMessageString($"Erreur : {ex.Message}");
}
finally
{
    // Toujours cacher la barre de progression
    statusBar.HideProgressMeter();
}
```

## Meilleures Pratiques

1. **Toujours Cacher la Barre de Progression** : Utilisez try-finally pour vous assurer que la barre de progression est cachée même si une erreur se produit.
2. **Fréquence de Mise à Jour** : Ne mettez pas à jour trop fréquemment (cause des scintillements), mettre à jour tous les 1-5% est généralement suffisant.
3. **Messages Clairs** : Définissez un message final lorsque l'opération se termine.
4. **Retour Utilisateur** : Fournissez des messages significatifs qui décrivent ce qui se passe.

## Objets Associés
- [Application](Application.md) - Fournit l'accès à StatusBar

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_StatusBar)
