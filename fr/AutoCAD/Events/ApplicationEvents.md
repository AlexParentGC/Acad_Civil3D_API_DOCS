# Application Events

**Namespace:** `Autodesk.AutoCAD.ApplicationServices`  
**Assembly:** `acmgd.dll`

## Vue d'Ensemble

La classe `Application` expose des événements au niveau de l'application qui se déclenchent lorsque des documents sont créés, ouverts, fermés ou lorsque l'état de l'application change. Ce sont des événements globaux qui s'appliquent à tous les documents.

**Concept Clé:** Les événements d'application surveillent l'application AutoCAD elle-même, pas les documents individuels. Utilisez-les pour les opérations à l'échelle de l'application comme la gestion des collections de documents ou la réponse aux changements au niveau du système.

## Événements Clés

| Événement | Description |
|-----------|-------------|
| `DocumentCreated` | Se déclenche lorsqu'un nouveau document est créé |
| `DocumentToBeDestroyed` | Se déclenche avant qu'un document ne soit détruit |
| `DocumentDestroyed` | Se déclenche après qu'un document est détruit |
| `DocumentActivated` | Se déclenche lorsqu'un document devient actif |
| `DocumentToBeActivated` | Se déclenche avant qu'un document ne devienne actif |
| `DocumentToBeDeactivated` | Se déclenche avant qu'un document ne soit désactivé |
| `DocumentBecameCurrent` | Se déclenche lorsqu'un document devient courant |
| `BeginQuit` | Se déclenche avant qu'AutoCAD ne quitte |
| `QuitAborted` | Se déclenche lorsque la fermeture est annulée |
| `QuitWillStart` | Se déclenche lorsque la fermeture va commencer |
| `SystemVariableChanged` | Se déclenche lorsqu'une variable système change |
| `SystemVariableWillChange` | Se déclenche avant qu'une variable système ne change |

## Modèles d'Utilisation Courants

### 1. Surveillance du Cycle de Vie des Documents

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Runtime;

public class ApplicationEventMonitor
{
    public void RegisterEvents()
    {
        Application.DocumentManager.DocumentCreated += OnDocumentCreated;
        Application.DocumentManager.DocumentToBeDestroyed += OnDocumentToBeDestroyed;
        Application.DocumentManager.DocumentDestroyed += OnDocumentDestroyed;
    }
    
    public void UnregisterEvents()
    {
        Application.DocumentManager.DocumentCreated -= OnDocumentCreated;
        Application.DocumentManager.DocumentToBeDestroyed -= OnDocumentToBeDestroyed;
        Application.DocumentManager.DocumentDestroyed -= OnDocumentDestroyed;
    }
    
    private void OnDocumentCreated(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Document Créé : {e.Document.Name}");
    }
    
    private void OnDocumentToBeDestroyed(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Document Sera Détruit : {e.Document.Name}");
    }
    
    private void OnDocumentDestroyed(object sender, DocumentDestroyedEventArgs e)
    {
        // Note : Le document est déjà détruit, impossible d'accéder à l'Editor
        System.Diagnostics.Debug.WriteLine($"Document Détruit : {e.FileName}");
    }
}
```

### 2. Suivi de l'Activation des Documents

```csharp
public class DocumentActivationTracker
{
    private int _activationCount = 0;
    
    public void Start()
    {
        Application.DocumentManager.DocumentActivated += OnDocumentActivated;
        Application.DocumentManager.DocumentToBeActivated += OnDocumentToBeActivated;
        Application.DocumentManager.DocumentToBeDeactivated += OnDocumentToBeDeactivated;
    }
    
    private void OnDocumentToBeActivated(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Va Activer : {e.Document.Name}");
    }
    
    private void OnDocumentActivated(object sender, DocumentCollectionEventArgs e)
    {
        _activationCount++;
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Activé : {e.Document.Name} (Compte : {_activationCount})");
    }
    
    private void OnDocumentToBeDeactivated(object sender, DocumentCollectionEventArgs e)
    {
        Editor ed = e.Document.Editor;
        ed.WriteMessage($"\n>>> Va Désactiver : {e.Document.Name}");
    }
}
```

### 3. Surveillance des Changements de Variables Système

```csharp
public class SysVarMonitor
{
    public void Start()
    {
        Application.SystemVariableWillChange += OnSysVarWillChange;
        Application.SystemVariableChanged += OnSysVarChanged;
    }
    
    private void OnSysVarWillChange(object sender, SystemVariableChangingEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Variable Système Va Changer : {e.Name}");
        ed.WriteMessage($"\n    Valeur Actuelle : {Application.GetSystemVariable(e.Name)}");
    }
    
    private void OnSysVarChanged(object sender, SystemVariableChangedEventArgs e)
    {
        Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
        ed.WriteMessage($"\n>>> Variable Système Changée : {e.Name}");
        ed.WriteMessage($"\n    Nouvelle Valeur : {Application.GetSystemVariable(e.Name)}");
    }
}
```

### 4. Gestionnaire de Fermeture d'Application

```csharp
public class QuitHandler
{
    public void Start()
    {
        Application.QuitWillStart += OnQuitWillStart;
        Application.QuitAborted += OnQuitAborted;
        Application.BeginQuit += OnBeginQuit;
    }
    
    private void OnBeginQuit(object sender, EventArgs e)
    {
        System.Diagnostics.Debug.WriteLine(">>> AutoCAD commence à quitter");
        
        // Effectuer des opérations de nettoyage
        CleanupResources();
    }
    
    private void OnQuitWillStart(object sender, EventArgs e)
    {
        System.Diagnostics.Debug.WriteLine(">>> La fermeture d'AutoCAD va commencer");
    }
    
    private void OnQuitAborted(object sender, EventArgs e)
    {
        System.Diagnostics.Debug.WriteLine(">>> La fermeture d'AutoCAD a été annulée");
    }
    
    private void CleanupResources()
    {
        // Fermer les fichiers journaux, sauvegarder les paramètres, etc.
    }
}
```

## Meilleures Pratiques

1. **À l'échelle de l'application uniquement**: Utiliser pour les opérations inter-documents
2. **Toujours désenregistrer**: Critique pour les événements d'application
3. **Traitement minimal**: Garder les gestionnaires très rapides
4. **Thread-safe**: Les événements d'application peuvent se déclencher sur différents threads
5. **Attention à l'accès aux documents**: Certains événements se déclenchent lorsque les documents ne sont pas disponibles

## Modèles Communs

### Modèle 1: Cycle de Vie des Documents
```csharp
Application.DocumentManager.DocumentCreated += OnCreated;
Application.DocumentManager.DocumentDestroyed += OnDestroyed;
```

### Modèle 2: Suivi d'Activation
```csharp
Application.DocumentManager.DocumentActivated += OnActivated;
Application.DocumentManager.DocumentToBeDeactivated += OnDeactivated;
```

### Modèle 3: Nettoyage d'Application
```csharp
Application.BeginQuit += (s, e) => {
    // Nettoyer les ressources
};
```

## Classes Associées

- [Application](../Core/Application.md) - Expose ces événements
- [DocumentManager](../Core/DocumentManager.md) - Gestionnaire de collection de documents
- [Document](../Core/Document.md) - Document individuel
- DocumentCollectionEventArgs - Arguments d'événement

## Voir Aussi

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-ApplicationEvents)
