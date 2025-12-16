# Classe DBDictionary

## Vue d'Ensemble
La classe `DBDictionary` est un objet conteneur qui stocke des entrées nommées d'autres objets de base de données. Les dictionnaires fournissent une structure hiérarchique pour organiser les données personnalisées, les paramètres et les objets dans un dessin AutoCAD. Le dictionnaire le plus important est le Dictionnaire des Objets Nommés (NOD), accessible depuis la base de données.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ DBDictionary
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Count` | `int` | Obtient le nombre d'entrées dans le dictionnaire |
| `TreatElementsAsHard` | `bool` | Obtient/définit si les entrées sont détenues "hard" ou "soft" |
| `IsClonable` | `bool` | Obtient si le dictionnaire peut être cloné |
| `MergeStyle` | `DictionaryMergeStyle` | Obtient/définit comment le dictionnaire est fusionné durant WBLOCK |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SetAt(string, DBObject)` | `ObjectId` | Ajoute ou remplace une entrée dans le dictionnaire |
| `GetAt(string)` | `ObjectId` | Obtient l'ObjectId d'une entrée par nom de clé |
| `Contains(string)` | `bool` | Vérifie si une clé existe dans le dictionnaire |
| `Remove(string)` | `void` | Supprime une entrée du dictionnaire |
| `GetEnumerator()` | `IEnumerator` | Obtient un énumérateur pour itérer les entrées |

## Exemples de Code

### Exemple 1: Accéder au Dictionnaire des Objets Nommés
```csharp
Document doc = Application.DocumentManager.MdiActiveDocument;
Database db = doc.Database;
Editor ed = doc.Editor;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir le Dictionnaire des Objets Nommés (NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage($"\n=== Dictionnaire Objets Nommés ===");
    ed.WriteMessage($"\nTotal entrées : {nod.Count}");
    ed.WriteMessage($"\nTraiter éléments comme hard : {nod.TreatElementsAsHard}");
    
    // Lister toutes les entrées de premier niveau
    ed.WriteMessage("\n\nEntrées de premier niveau :");
    foreach (DBDictionaryEntry entry in nod)
    {
        DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
        ed.WriteMessage($"\n  {entry.Key} ({obj.GetType().Name})");
    }
    
    tr.Commit();
}
```

### Exemple 2: Créer un Dictionnaire Personnalisé
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    string dictName = "MyCompanyData";
    
    // Vérifier si le dictionnaire existe déjà
    if (!nod.Contains(dictName))
    {
        // Créer nouveau dictionnaire
        DBDictionary customDict = new DBDictionary();
        
        // Ajouter au NOD
        ObjectId dictId = nod.SetAt(dictName, customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
        
        ed.WriteMessage($"\nDictionnaire personnalisé créé : {dictName}");
        ed.WriteMessage($"\nObjectId du dictionnaire : {dictId}");
    }
    else
    {
        ed.WriteMessage($"\nDictionnaire '{dictName}' existe déjà");
    }
    
    tr.Commit();
}
```

### Exemple 3: Ajouter des Entrées à un Dictionnaire
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    // Obtenir ou créer dictionnaire personnalisé
    DBDictionary customDict;
    if (nod.Contains("MyCompanyData"))
    {
        customDict = tr.GetObject(nod.GetAt("MyCompanyData"), OpenMode.ForWrite) as DBDictionary;
    }
    else
    {
        nod.UpgradeOpen();
        customDict = new DBDictionary();
        nod.SetAt("MyCompanyData", customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
    }
    
    // Créer et ajouter des XRecords
    string[] recordNames = { "ProjectInfo", "Settings", "Metadata" };
    
    foreach (string recordName in recordNames)
    {
        if (!customDict.Contains(recordName))
        {
            XRecord xRec = new XRecord();
            ResultBuffer rb = new ResultBuffer(
                new TypedValue((int)DxfCode.Text, recordName),
                new TypedValue((int)DxfCode.Text, DateTime.Now.ToString()),
                new TypedValue((int)DxfCode.Int32, 1)
            );
            xRec.Data = rb;
            
            customDict.SetAt(recordName, xRec);
            tr.AddNewlyCreatedDBObject(xRec, true);
            
            rb.Dispose();
            
            ed.WriteMessage($"\nEntrée ajoutée : {recordName}");
        }
    }
    
    tr.Commit();
}
```

### Exemple 4: Énumérer le Contenu du Dictionnaire
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyCompanyData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyCompanyData"), OpenMode.ForRead) as DBDictionary;
        
        ed.WriteMessage($"\n=== Dictionnaire MyCompanyData ===");
        ed.WriteMessage($"\nEntrées : {customDict.Count}");
        
        // Méthode 1: Utiliser foreach avec DBDictionaryEntry
        foreach (DBDictionaryEntry entry in customDict)
        {
            DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
            ed.WriteMessage($"\n  Clé : {entry.Key}");
            ed.WriteMessage($"\n    Type : {obj.GetType().Name}");
            ed.WriteMessage($"\n    ObjectId : {entry.Value}");
            
            if (obj is XRecord)
            {
                XRecord xRec = obj as XRecord;
                ResultBuffer rb = xRec.Data;
                if (rb != null)
                {
                    ed.WriteMessage($"\n    Éléments de données : {rb.AsArray().Length}");
                    rb.Dispose();
                }
            }
        }
    }
    
    tr.Commit();
}
```

### Exemple 5: Créer des Dictionnaires Imbriqués
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Créer dictionnaire principal
    DBDictionary mainDict = new DBDictionary();
    nod.SetAt("ProjectData", mainDict);
    tr.AddNewlyCreatedDBObject(mainDict, true);
    
    // Créer dictionnaires imbriqués
    string[] categories = { "Structural", "Mechanical", "Electrical" };
    
    foreach (string category in categories)
    {
        DBDictionary categoryDict = new DBDictionary();
        mainDict.SetAt(category, categoryDict);
        tr.AddNewlyCreatedDBObject(categoryDict, true);
        
        // Ajouter XRecords à chaque catégorie
        XRecord xRec = new XRecord();
        ResultBuffer rb = new ResultBuffer(
            new TypedValue((int)DxfCode.Text, $"{category} Data"),
            new TypedValue((int)DxfCode.Real, 100.0)
        );
        xRec.Data = rb;
        
        categoryDict.SetAt("Info", xRec);
        tr.AddNewlyCreatedDBObject(xRec, true);
        
        rb.Dispose();
        
        ed.WriteMessage($"\nCatégorie créée : {category}");
    }
    
    tr.Commit();
}
```

### Exemple 6: Travailler avec les Dictionnaires Standard
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Accéder au Dictionnaire de Groupes
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, OpenMode.ForRead) as DBDictionary;
    ed.WriteMessage($"\n=== Groupes ===");
    ed.WriteMessage($"\nTotal groupes : {groupDict.Count}");
    
    foreach (DBDictionaryEntry entry in groupDict)
    {
        Group group = tr.GetObject(entry.Value, OpenMode.ForRead) as Group;
        ed.WriteMessage($"\n  {entry.Key}: {group.Count} objets");
    }
    
    // Accéder au Dictionnaire de Présentations
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    ed.WriteMessage($"\n\n=== Présentations ===");
    ed.WriteMessage($"\nTotal présentations : {layoutDict.Count}");
    
    foreach (DBDictionaryEntry entry in layoutDict)
    {
        Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
        ed.WriteMessage($"\n  {layout.LayoutName} (Type Modèle : {layout.ModelType})");
    }
    
    // Accéder au Dictionnaire de Matériaux
    DBDictionary materialDict = tr.GetObject(db.MaterialDictionaryId, OpenMode.ForRead) as DBDictionary;
    ed.WriteMessage($"\n\n=== Matériaux ===");
    ed.WriteMessage($"\nTotal matériaux : {materialDict.Count}");
    
    tr.Commit();
}
```

### Exemple 7: Travailler avec les Dictionnaires d'Extension d'Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir une entité
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Créer dictionnaire d'extension s'il n'existe pas
    if (ent.ExtensionDictionary == ObjectId.Null)
    {
        ent.CreateExtensionDictionary();
        ed.WriteMessage("\nDictionnaire d'extension créé");
    }
    
    // Accéder au dictionnaire d'extension
    DBDictionary extDict = tr.GetObject(ent.ExtensionDictionary, OpenMode.ForWrite) as DBDictionary;
    
    // Ajouter données personnalisées
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "EntityMetadata"),
        new TypedValue((int)DxfCode.Text, "Auteur"),
        new TypedValue((int)DxfCode.Text, "Jean Dupont"),
        new TypedValue((int)DxfCode.Text, "Date"),
        new TypedValue((int)DxfCode.Text, DateTime.Now.ToString("yyyy-MM-dd"))
    );
    xRec.Data = rb;
    
    extDict.SetAt("Metadata", xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    ed.WriteMessage($"\nMétadonnées attachées à {ent.GetType().Name}");
    ed.WriteMessage($"\nEntrées dictionnaire d'extension : {extDict.Count}");
    
    tr.Commit();
}
```

### Exemple 8: Supprimer des Entrées de Dictionnaire
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyCompanyData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyCompanyData"), OpenMode.ForWrite) as DBDictionary;
        
        // Supprimer une entrée spécifique
        string entryToRemove = "Settings";
        if (customDict.Contains(entryToRemove))
        {
            customDict.Remove(entryToRemove);
            ed.WriteMessage($"\nEntrée supprimée : {entryToRemove}");
        }
        
        // Supprimer toutes les entrées
        List<string> keysToRemove = new List<string>();
        foreach (DBDictionaryEntry entry in customDict)
        {
            keysToRemove.Add(entry.Key);
        }
        
        foreach (string key in keysToRemove)
        {
            customDict.Remove(key);
            ed.WriteMessage($"\nSupprimé : {key}");
        }
        
        ed.WriteMessage($"\nEntrées restantes : {customDict.Count}");
    }
    
    tr.Commit();
}
```

### Exemple 9: Propriété Hard vs Soft
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Créer dictionnaire avec propriété hard (défaut)
    DBDictionary hardDict = new DBDictionary();
    hardDict.TreatElementsAsHard = true;  // Entrées sont hard-owned (supprimées avec dictionnaire)
    nod.SetAt("HardOwnedDict", hardDict);
    tr.AddNewlyCreatedDBObject(hardDict, true);
    
    // Créer dictionnaire avec propriété soft
    DBDictionary softDict = new DBDictionary();
    softDict.TreatElementsAsHard = false;  // Entrées sont soft-owned (non supprimées avec dictionnaire)
    nod.SetAt("SoftOwnedDict", softDict);
    tr.AddNewlyCreatedDBObject(softDict, true);
    
    ed.WriteMessage("\nDictionnaires créés avec propriété différente :");
    ed.WriteMessage($"\n  HardOwnedDict : TreatElementsAsHard = {hardDict.TreatElementsAsHard}");
    ed.WriteMessage($"\n  SoftOwnedDict : TreatElementsAsHard = {softDict.TreatElementsAsHard}");
    
    tr.Commit();
}
```

### Exemple 10: Rechercher Dictionnaires Imbriqués Récursivement
```csharp
public void SearchDictionariesRecursive(ObjectId dictId, Transaction tr, Editor ed, int level = 0)
{
    DBDictionary dict = tr.GetObject(dictId, OpenMode.ForRead) as DBDictionary;
    string indent = new string(' ', level * 2);
    
    foreach (DBDictionaryEntry entry in dict)
    {
        DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
        
        ed.WriteMessage($"\n{indent}{entry.Key} ({obj.GetType().Name})");
        
        if (obj is DBDictionary)
        {
            // Recherche récursive
            SearchDictionariesRecursive(entry.Value, tr, ed, level + 1);
        }
        else if (obj is XRecord)
        {
            XRecord xRec = obj as XRecord;
            ResultBuffer rb = xRec.Data;
            if (rb != null)
            {
                ed.WriteMessage($" - {rb.AsArray().Length} éléments");
                rb.Dispose();
            }
        }
    }
}

// Usage:
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ed.WriteMessage("\n=== Hiérarchie Dictionnaire ===");
    SearchDictionariesRecursive(db.NamedObjectsDictionaryId, tr, ed);
    tr.Commit();
}
```

## Dictionnaires de Base de Données Standard

La classe Database fournit un accès à plusieurs dictionnaires standard :

| Propriété | But du Dictionnaire |
|-----------|---------------------|
| `NamedObjectsDictionaryId` | Dictionnaire racine pour tous les objets nommés |
| `GroupDictionaryId` | Groupes d'objets |
| `LayoutDictionaryId` | Présentations espace papier |
| `MLStyleDictionaryId` | Styles multilignes |
| `PlotSettingsDictionaryId` | Configurations de tracé |
| `ColorDictionaryId` | Définitions de couleurs |
| `MaterialDictionaryId` | Définitions de matériaux |
| `VisualStyleDictionaryId` | Styles visuels |

## Propriété de Dictionnaire (Ownership)

### Hard Ownership (`TreatElementsAsHard = true`)
- Dictionnaire **possède** ses entrées
- Quand le dictionnaire est effacé, toutes les entrées sont effacées
- Les entrées ne peuvent exister sans le dictionnaire
- **Défaut pour la plupart des dictionnaires**

### Soft Ownership (`TreatElementsAsHard = false`)
- Dictionnaire **référence** ses entrées
- Quand le dictionnaire est effacé, les entrées restent
- Les entrées peuvent exister indépendamment
- Utilisé quand plusieurs dictionnaires référencent les mêmes objets

## Meilleures Pratiques

1. **Vérifier l'Existence** : Utilisez toujours `Contains()` avant d'accéder aux entrées
2. **Gestion de Transaction** : Utilisez toujours des transactions lors de la modification de dictionnaires
3. **Conventions de Nommage** : Utilisez des noms descriptifs et uniques pour les entrées
4. **Organisation Hiérarchique** : Utilisez des dictionnaires imbriqués pour organiser les structures complexes
5. **Dictionnaires d'Extension** : Utilisez les dictionnaires d'extension pour les données spécifiques aux entités
6. **Conscience de Propriété** : Comprenez les implications de hard vs soft ownership
7. **Sécurité d'Énumération** : Collectez les clés avant de supprimer des entrées pendant l'énumération
8. **Libérer les Ressources** : Libérez les ResultBuffers lors de la lecture de XRecord

## Objets Associés
- [XRecord](XRecord.md) - Objet de stockage de données personnalisé
- [Database](Database.md) - Fournit l'accès aux dictionnaires standard
- [Entity](../BaseClasses/Entity.md) - Peut avoir des dictionnaires d'extension
- [Group](Group.md) - Stocké dans le dictionnaire de groupes
- [Layout](Layout.md) - Stocké dans le dictionnaire de présentations

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-A809CD71-4655-44E2-B674-1FE200B9FE75)
- [Référence Classe DBDictionary](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBDictionary)
