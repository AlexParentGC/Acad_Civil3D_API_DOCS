# Document Events

**Namespace:** `Autodesk.AutoCAD.ApplicationServices`  
**Assembly:** `acmgd.dll`

## Vue d'Ensemble

La classe `Document` expose des événements qui se déclenchent pendant les opérations du cycle de vie du document telles que l'activation, la désactivation, l'exécution de commandes et les changements d'état de fenêtre. Ces événements vous permettent de répondre aux interactions utilisateur et aux changements d'état du document.

**Concept Clé:** Les événements de document suivent le cycle de vie et l'état des documents de dessin individuels, vous permettant d'exécuter du code lorsque les documents sont ouverts, fermés, activés ou lorsque des commandes sont exécutées.

## Événements Clés

| Événement | Description |
|-----------|-------------|
| `CommandWillStart` | Se déclenche avant le démarrage d'une commande |
| `CommandEnded` | Se déclenche après la fin d'une commande |
| `CommandCancelled` | Se déclenche lorsqu'une commande est annulée |
| `CommandFailed` | Se déclenche lorsqu'une commande échoue |
| `LispWillStart` | Se déclenche avant l'évaluation d'une expression LISP |
| `LispEnded` | Se déclenche après la fin d'une expression LISP |
| `LispCancelled` | Se déclenche lorsque l'évaluation LISP est annulée |
| `BeginDocumentClose` | Se déclenche avant la fermeture du document |
| `DocumentLockModeWillChange` | Se déclenche avant le changement de mode de verrouillage du document |
| `DocumentLockModeChanged` | Se déclenche après le changement de mode de verrouillage du document |
| `ImpliedSelectionChanged` | Se déclenche lorsque la sélection pickfirst change |

## Modèles d'Utilisation Courants

### 1. Surveillance de l'Exécution des Commandes

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Runtime;

public class CommandMonitor
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        
        _doc.CommandWillStart += OnCommandWillStart;
        _doc.CommandEnded += OnCommandEnded;
        _doc.CommandCancelled += OnCommandCancelled;
        _doc.CommandFailed += OnCommandFailed;
    }
    
    public void Stop()
    {
        if (_doc != null)
        {
            _doc.CommandWillStart -= OnCommandWillStart;
            _doc.CommandEnded -= OnCommandEnded;
            _doc.CommandCancelled -= OnCommandCancelled;
            _doc.CommandFailed -= OnCommandFailed;
        }
    }
    
    private void OnCommandWillStart(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Commande Démarrage : {e.GlobalCommandName}");
    }
    
    private void OnCommandEnded(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Commande Terminée : {e.GlobalCommandName}");
    }
    
    private void OnCommandCancelled(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Commande Annulée : {e.GlobalCommandName}");
    }
    
    private void OnCommandFailed(object sender, CommandEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> Commande Échouée : {e.GlobalCommandName}");
    }
}
```

### 2. Chronométrage et Suivi des Performances des Commandes

```csharp
public class CommandPerformanceTracker
{
    private Document _doc;
    private System.Diagnostics.Stopwatch _stopwatch;
    private string _currentCommand;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        _stopwatch = new System.Diagnostics.Stopwatch();
        
        _doc.CommandWillStart += OnCommandWillStart;
        _doc.CommandEnded += OnCommandEnded;
    }
    
    private void OnCommandWillStart(object sender, CommandEventArgs e)
    {
        _currentCommand = e.GlobalCommandName;
        _stopwatch.Restart();
    }
    
    private void OnCommandEnded(object sender, CommandEventArgs e)
    {
        _stopwatch.Stop();
        double seconds = _stopwatch.Elapsed.TotalSeconds;
        
        _doc.Editor.WriteMessage(
            $"\n>>> {_currentCommand} terminée en {seconds:F3} secondes");
    }
}
```

### 3. Prévention de la Fermeture du Document

```csharp
public class CloseGuard
{
    private Document _doc;
    private bool _allowClose = false;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        _doc.BeginDocumentClose += OnBeginDocumentClose;
    }
    
    private void OnBeginDocumentClose(object sender, DocumentBeginCloseEventArgs e)
    {
        if (!_allowClose)
        {
            // Vérifier s'il y a des données personnalisées non sauvegardées
            bool hasUnsavedData = CheckForUnsavedData();
            
            if (hasUnsavedData)
            {
                System.Windows.Forms.DialogResult result = 
                    System.Windows.Forms.MessageBox.Show(
                        "Vous avez des données personnalisées non sauvegardées. Fermer quand même ?",
                        "Avertissement",
                        System.Windows.Forms.MessageBoxButtons.YesNo,
                        System.Windows.Forms.MessageBoxIcon.Warning);
                
                if (result == System.Windows.Forms.DialogResult.No)
                {
                    e.Veto(); // Empêcher la fermeture
                }
            }
        }
    }
    
    private bool CheckForUnsavedData()
    {
        // Votre logique personnalisée ici
        return false;
    }
}
```

### 4. Surveillance des Expressions LISP

```csharp
public class LispMonitor
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        
        _doc.LispWillStart += OnLispWillStart;
        _doc.LispEnded += OnLispEnded;
        _doc.LispCancelled += OnLispCancelled;
    }
    
    private void OnLispWillStart(object sender, LispWillStartEventArgs e)
    {
        _doc.Editor.WriteMessage($"\n>>> LISP Démarrage : {e.FirstLine}");
    }
    
    private void OnLispEnded(object sender, LispEndedEventArgs e)
    {
        _doc.Editor.WriteMessage("\n>>> LISP Terminé");
    }
    
    private void OnLispCancelled(object sender, EventArgs e)
    {
        _doc.Editor.WriteMessage("\n>>> LISP Annulé");
    }
}
```

### 5. Suivi du Mode de Verrouillage du Document

```csharp
public class LockModeTracker
{
    private Document _doc;
    
    public void Start()
    {
        _doc = Application.DocumentManager.MdiActiveDocument;
        
        _doc.DocumentLockModeWillChange += OnLockModeWillChange;
        _doc.DocumentLockModeChanged += OnLockModeChanged;
    }
    
    private void OnLockModeWillChange(object sender, DocumentLockModeWillChangeEventArgs e)
    {
        _doc.Editor.WriteMessage(
            $"\n>>> Mode de Verrouillage Va Changer : {e.CurrentMode} → {e.NewMode}");
        
        _doc.Editor.WriteMessage(
            $"\n    Commande Globale : {e.GlobalCommandName}");
    }
    
    private void OnLockModeChanged(object sender, DocumentLockModeChangedEventArgs e)
    {
        _doc.Editor.WriteMessage(
            $"\n>>> Mode de Verrouillage Changé : {e.CurrentMode}");
    }
}
```

## Meilleures Pratiques

1. **Événements spécifiques au document**: Enregistrer les événements par document, pas globalement
2. **Toujours désenregistrer**: Supprimer les gestionnaires d'événements lorsque terminé
3. **Éviter le traitement lourd**: Garder les gestionnaires d'événements légers
4. **Utiliser Veto avec précaution**: Utiliser le veto uniquement lorsque absolument nécessaire
5. **Gérer les exceptions**: Envelopper le code d'événement dans des blocs try-catch

## Modèles Communs

### Modèle 1: Cycle de Vie des Commandes
```csharp
_doc.CommandWillStart += OnStart;
_doc.CommandEnded += OnEnd;
_doc.CommandCancelled += OnCancel;
_doc.CommandFailed += OnFail;
```

### Modèle 2: Protection du Document
```csharp
_doc.BeginDocumentClose += (s, e) => {
    if (NeedsSave()) e.Veto();
};
```

## Classes Associées

- [Document](../Core/Document.md) - Expose ces événements
- [DocumentCollection](../Core/DocumentManager.md) - Événements de collection de documents
- CommandEventArgs - Arguments d'événement de commande
- [Editor](../Core/Editor.md) - Interaction utilisateur

## Voir Aussi

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-DocumentEvents)
