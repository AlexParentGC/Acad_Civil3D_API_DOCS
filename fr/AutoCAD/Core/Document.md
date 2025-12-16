# Classe Document

## Vue d'Ensemble
La classe `Document` représente un document de dessin AutoCAD ouvert. Elle fournit l'accès à la base de données, l'éditeur et les opérations au niveau du document.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ Document
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Database` | `Database` | Obtient la base de données du document |
| `Editor` | `Editor` | Obtient l'éditeur du document |
| `Name` | `string` | Obtient le nom du document (nom de fichier) |
| `Window` | `Window` | Obtient la fenêtre du document |
| `UserData` | `IDictionary` | Obtient le dictionnaire de données définies par l'utilisateur |
| `IsReadOnly` | `bool` | Indique si le document est en lecture seule |
| `IsActive` | `bool` | Indique si le document est actif |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `LockDocument()` | `DocumentLock` | Verrouille le document pour modifications |
| `LockDocument(DocumentLockMode, string, string, bool)` | `DocumentLock` | Verrouille avec un mode spécifique |
| `CloseAndDiscard()` | `void` | Ferme le document sans sauvegarder |
| `CloseAndSave(string)` | `void` | Ferme et sauvegarde le document |
| `SendStringToExecute(string, bool, bool, bool)` | `void` | Exécute une chaîne de commande |

## Exemples de Code

### Exemple 1: Accéder aux Propriétés du Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;

ed.WriteMessage($"\nNom du Document : {acDoc.Name}");
ed.WriteMessage($"\nEst Actif : {acDoc.IsActive}");
ed.WriteMessage($"\nEst Lecture Seule : {acDoc.IsReadOnly}");

Database db = acDoc.Database;
Editor ed = acDoc.Editor;
```

### Exemple 2: Verrouiller le Document pour Modifications
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
Database db = acDoc.Database;

// Verrouiller le document avant de modifier
using (DocumentLock docLock = acDoc.LockDocument())
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Modifier les objets de base de données ici
        
        tr.Commit();
    }
}
```

### Exemple 3: Exécuter des Commandes
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;

// Exécuter une commande
acDoc.SendStringToExecute("ZOOM E ", true, false, false);
```

### Exemple 4: Sauvegarder le Document
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;
Database db = acDoc.Database;

// Sauvegarder le document courant
db.SaveAs(acDoc.Name, DwgVersion.Current);

// Ou sauvegarder avec un nouveau nom
db.SaveAs("C:\\Dessins\\NouveauNom.dwg", DwgVersion.Current);
```

### Exemple 5: Utiliser les Données Utilisateur
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Document acDoc = Application.DocumentManager.MdiActiveDocument;

// Stocker des données personnalisées avec le document
acDoc.UserData["MaCle"] = "MaValeur";

// Récupérer des données personnalisées
if (acDoc.UserData.Contains("MaCle"))
{
    string value = acDoc.UserData["MaCle"] as string;
    ed.WriteMessage($"\nValeur stockée : {value}");
}
```

## Modes de Verrouillage de Document

| Mode | Description |
|------|-------------|
| `NotLocked` | Document n'est pas verrouillé |
| `AutoWrite` | Verrouillage écriture automatique |
| `ProtectedAutoWrite` | Verrouillage écriture automatique protégé |
| `Read` | Verrouillage lecture seule |
| `Write` | Verrouillage écriture |

## Objets Associés
- [Application](Application.md) - Objet application racine
- [DocumentManager](DocumentManager.md) - Gère les documents
- [Database](Database.md) - Base de données du document
- [Editor](Editor.md) - Interaction utilisateur

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_Document)
