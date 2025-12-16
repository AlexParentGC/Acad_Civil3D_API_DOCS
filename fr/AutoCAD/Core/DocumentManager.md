# Classe DocumentManager

## Vue d'Ensemble
La classe `DocumentManager` gère tous les documents ouverts dans AutoCAD. Elle fournit l'accès au document actif et permet l'itération à travers tous les documents ouverts.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ DocumentManager
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `MdiActiveDocument` | `Document` | Obtient le document actuellement actif |
| `CurrentDocument` | `Document` | Obtient le contexte de document courant |
| `Count` | `int` | Obtient le nombre de documents ouverts |

## Événements Clés

| Événement | Description |
|-----------|-------------|
| `DocumentCreated` | Déclenché lorsqu'un nouveau document est créé |
| `DocumentToBeDestroyed` | Déclenché avant qu'un document ne soit fermé |
| `DocumentActivated` | Déclenché lorsqu'un document devient actif |
| `DocumentBecameCurrent` | Déclenché lorsqu'un document devient courant |

## Exemples de Code

### Exemple 1: Accéder au Document Actif
```csharp
using Autodesk.AutoCAD.ApplicationServices;

DocumentManager docMgr = Application.DocumentManager;
Document acDoc = docMgr.MdiActiveDocument;

if (acDoc != null)
{
    Editor ed = acDoc.Editor;
    ed.WriteMessage($"\nDocument actif : {acDoc.Name}");
}
```

### Exemple 2: Itérer Travers Tous les Documents
```csharp
using Autodesk.AutoCAD.ApplicationServices;

DocumentManager docMgr = Application.DocumentManager;

ed.WriteMessage($"\nTotal documents ouverts : {docMgr.Count}");

foreach (Document doc in docMgr)
{
    ed.WriteMessage($"\n  {doc.Name}");
}
```

### Exemple 3: Créer un Nouveau Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;

DocumentManager docMgr = Application.DocumentManager;

// Créer un nouveau document depuis un modèle
Document newDoc = docMgr.Add("acad.dwt");

// Le rendre actif
docMgr.MdiActiveDocument = newDoc;
```

### Exemple 4: Gérer les Événements de Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;

public void RegisterDocumentEvents()
{
    DocumentManager docMgr = Application.DocumentManager;
    
    docMgr.DocumentCreated += OnDocumentCreated;
    docMgr.DocumentToBeDestroyed += OnDocumentToBeDestroyed;
}

private void OnDocumentCreated(object sender, DocumentCollectionEventArgs e)
{
    Document doc = e.Document;
    Editor ed = doc.Editor;
    ed.WriteMessage($"\nDocument créé : {doc.Name}");
}

private void OnDocumentToBeDestroyed(object sender, DocumentCollectionEventArgs e)
{
    Document doc = e.Document;
    // Nettoyage avant fermeture du document
}
```

## Objets Associés
- [Application](Application.md) - Fournit l'accès à DocumentManager
- [Document](Document.md) - Document individuel
- [Database](Database.md) - Base de données du document

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_DocumentManager)
