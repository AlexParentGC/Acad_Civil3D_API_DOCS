# CommandMethodAttribute Class

## Vue d'Ensemble
La classe `CommandMethodAttribute` est utilisée pour marquer les méthodes comme commandes AutoCAD. Cet attribut permet aux méthodes d'être invoquées depuis la ligne de commande AutoCAD.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Constructeur

| Constructeur | Description |
|--------------|-------------|
| `CommandMethodAttribute(string)` | Crée une commande avec le nom spécifié |
| `CommandMethodAttribute(string, CommandFlags)` | Crée une commande avec nom et drapeaux |

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `GlobalName` | `string` | Obtient ou définit le nom global de la commande |
| `LocalizedNameId` | `string` | Obtient ou définit l'ID de ressource du nom localisé |
| `GroupName` | `string` | Obtient ou définit le nom du groupe de commandes |
| `Flags` | `CommandFlags` | Obtient ou définit les drapeaux de commande |

## CommandFlags Énumération

| Drapeau | Description |
|---------|-------------|
| `Modal` | La commande s'exécute dans le contexte de l'application (par défaut) |
| `Transparent` | La commande peut s'exécuter pendant qu'une autre commande est active |
| `UsePickSet` | La commande utilise le jeu de sélection pickfirst actuel |
| `DocReadLock` | La commande nécessite un verrou de lecture du document |
| `DocExclusiveLock` | La commande nécessite un verrou exclusif du document |
| `Session` | La commande est à l'échelle de la session |
| `NoHistory` | La commande n'est pas ajoutée à l'historique des commandes |
| `NoUndoMarker` | La commande ne crée pas de marqueur d'annulation |

## Exemples de Code

### Commande de Base
```csharp
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;

[CommandMethod("HelloWorld")]
public void HelloWorldCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    
    ed.WriteMessage("\nBonjour, Monde AutoCAD !");
}
```

### Commande avec Drapeaux
```csharp
[CommandMethod("MyCommand", CommandFlags.Modal)]
public void MyCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    
    ed.WriteMessage("\nCeci est une commande modale");
}
```

### Commande Transparente
```csharp
[CommandMethod("ZoomExtents", CommandFlags.Transparent)]
public void ZoomExtentsCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    
    // Cette commande peut s'exécuter pendant qu'une autre commande est active
    doc.SendStringToExecute("._zoom _e ", true, false, false);
}
```

### Commande avec PickFirst
```csharp
[CommandMethod("ChangeColor", CommandFlags.UsePickSet)]
public void ChangeColorCommand()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    Database db = doc.Database;
    
    // Obtenir le jeu de sélection pickfirst
    PromptSelectionResult result = ed.SelectImplied();
    
    if (result.Status == PromptStatus.OK)
    {
        using (Transaction tr = db.TransactionManager.StartTransaction())
        {
            foreach (SelectedObject selObj in result.Value)
            {
                Entity ent = tr.GetObject(selObj.ObjectId, OpenMode.ForWrite) as Entity;
                ent.ColorIndex = 1; // Rouge
            }
            tr.Commit();
        }
        
        ed.WriteMessage($"\n{result.Value.Count} objets changés en rouge");
    }
}
```

## Meilleures Pratiques

1. **Noms de Commande**: Utiliser des noms de commande clairs et descriptifs
2. **Drapeaux**: Choisir les drapeaux appropriés pour le comportement de la commande
3. **Verrouillage**: Utiliser `DocReadLock` ou `DocExclusiveLock` lors de la modification de la base de données
4. **Modal**: La plupart des commandes devraient être Modal
5. **Transparent**: Utiliser avec parcimonie pour les commandes qui ne modifient pas le dessin
6. **PickFirst**: Utiliser `UsePickSet` pour les commandes qui fonctionnent avec des objets présélectionnés

## Classes Associées
- **LispFunctionAttribute** - Exposer les méthodes à LISP
- **CommandClass** - Enregistrement de commande
- **CommandFlags** - Drapeaux de comportement de commande
- **Editor** - Interaction ligne de commande

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
