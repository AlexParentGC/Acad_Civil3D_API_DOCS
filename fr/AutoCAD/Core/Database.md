# Classe Database

## Vue d'Ensemble
La classe `Database` représente une base de données de dessin AutoCAD. Elle est le dépôt central pour tous les objets graphiques et non-graphiques dans un dessin, incluant les entités, tables de symboles et dictionnaires.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Database
```

## Propriétés Clés

### Propriétés de Table de Symboles

| Propriété | Type | Description |
|-----------|------|-------------|
| `BlockTableId` | `ObjectId` | Obtient l'ObjectId de la BlockTable |
| `LayerTableId` | `ObjectId` | Obtient l'ObjectId de la LayerTable |
| `LinetypeTableId` | `ObjectId` | Obtient l'ObjectId de la LinetypeTable |
| `TextStyleTableId` | `ObjectId` | Obtient l'ObjectId de la TextStyleTable |
| `DimStyleTableId` | `ObjectId` | Obtient l'ObjectId de la DimStyleTable |
| `UcsTableId` | `ObjectId` | Obtient l'ObjectId de la UcsTable |
| `ViewTableId` | `ObjectId` | Obtient l'ObjectId de la ViewTable |
| `ViewportTableId` | `ObjectId` | Obtient l'ObjectId de la ViewportTable |
| `RegAppTableId` | `ObjectId` | Obtient l'ObjectId de la RegAppTable |

### Propriétés de Dictionnaire

| Propriété | Type | Description |
|-----------|------|-------------|
| `NamedObjectsDictionaryId` | `ObjectId` | Obtient le Dictionnaire des Objets Nommés (NOD) |
| `GroupDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Groupes |
| `MLStyleDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Styles Multilignes |
| `LayoutDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Présentations |
| `PlotSettingsDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Paramètres de Tracé |
| `ColorDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Couleurs |
| `MaterialDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Matériaux |
| `VisualStyleDictionaryId` | `ObjectId` | Obtient le Dictionnaire de Styles Visuels |

### Propriétés d'État Courant

| Propriété | Type | Description |
|-----------|------|-------------|
| `CurrentSpaceId` | `ObjectId` | Obtient l'ObjectId de l'espace courant (Objet ou Papier) |
| `Clayer` | `ObjectId` | Obtient/définit le calque courant |
| `Celtype` | `ObjectId` | Obtient/définit le type de ligne courant |
| `Dimstyle` | `ObjectId` | Obtient/définit le style de cote courant |
| `Textstyle` | `ObjectId` | Obtient/définit le style de texte courant |
| `Cmaterial` | `ObjectId` | Obtient/définit le matériau courant |

### Propriétés de Dessin

| Propriété | Type | Description |
|-----------|------|-------------|
| `TileMode` | `bool` | Obtient/définit si TileMode est actif (Espace Objet) |
| `Filename` | `string` | Obtient le chemin complet du fichier dessin |
| `OriginalFileName` | `string` | Obtient le nom de fichier original avant sauvegardes |
| `DwgVersion` | `DwgVersion` | Obtient la version du fichier DWG |
| `LastSavedAsVersion` | `DwgVersion` | Obtient la dernière version enregistrée |

### Propriétés Système

| Propriété | Type | Description |
|-----------|------|-------------|
| `TransactionManager` | `TransactionManager` | Obtient le gestionnaire de transaction pour cette base de données |
| `ObjectContextManager` | `ObjectContextManager` | Obtient le gestionnaire de contexte d'objet |
| `Dimscale` | `double` | Obtient/définit l'échelle de cote |
| `Ltscale` | `double` | Obtient/définit l'échelle de type de ligne |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `ReadDwgFile(string, FileOpenMode, bool, string)` | `Database` | Lit un fichier DWG dans un nouvel objet Database |
| `SaveAs(string, DwgVersion)` | `void` | Sauvegarde la base de données dans un fichier |
| `Audit(bool, bool)` | `void` | Audite la base de données pour les erreurs |
| `Purge(ObjectIdCollection)` | `void` | Purge les objets inutilisés |
| `GetObjectId(bool, Handle, int)` | `ObjectId` | Obtient un ObjectId depuis un descripteur (handle) |
| `GetAcadDatabase()` | `IntPtr` | Obtient un pointeur vers la AcDbDatabase sous-jacente |

## Exemples de Code

### Exemple 1: Accéder aux Tables de Symboles
```csharp
Document doc = Application.DocumentManager.MdiActiveDocument;
Database db = doc.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Accéder BlockTable
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Accéder LayerTable
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    // Accéder espace courant
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    tr.Commit();
}
```

### Exemple 2: Lire un Fichier DWG Externe
```csharp
using (Database extDb = new Database(false, true))
{
    extDb.ReadDwgFile("C:\\Dessins\\Exemple.dwg", FileOpenMode.OpenForReadAndAllShare, true, "");
    
    using (Transaction tr = extDb.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(extDb.BlockTableId, OpenMode.ForRead) as BlockTable;
        // Travailler avec base de données externe
        
        tr.Commit();
    }
}
```

### Exemple 3: Obtenir le Calque Courant
```csharp
Database db = Application.DocumentManager.MdiActiveDocument.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTableRecord currentLayer = tr.GetObject(db.Clayer, OpenMode.ForRead) as LayerTableRecord;
    
    Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
    ed.WriteMessage($"\nCalque courant : {currentLayer.Name}");
    
    tr.Commit();
}
```

### Exemple 4: Purger les Objets Inutilisés
```csharp
Database db = Application.DocumentManager.MdiActiveDocument.Database;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectIdCollection idsToPurge = new ObjectIdCollection();
    
    // Obtenir objets purgeables
    db.Purge(idsToPurge);
    
    if (idsToPurge.Count > 0)
    {
        // Purger les objets
        foreach (ObjectId id in idsToPurge)
        {
            DBObject obj = tr.GetObject(id, OpenMode.ForWrite);
            obj.Erase();
        }
    }
    
    tr.Commit();
}
```

### Exemple 5: Accéder au Dictionnaire des Objets Nommés
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir le Dictionnaire des Objets Nommés (NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\nEntrées du Dictionnaire d'Objets Nommés :");
    foreach (DBDictionaryEntry entry in nod)
    {
        ed.WriteMessage($"\n  {entry.Key}");
    }
    
    tr.Commit();
}
```

### Exemple 6: Accéder au Dictionnaire de Groupes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary groupDict = tr.GetObject(db.GroupDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage($"\nNombre de groupes : {groupDict.Count}");
    
    foreach (DBDictionaryEntry entry in groupDict)
    {
        Group group = tr.GetObject(entry.Value, OpenMode.ForRead) as Group;
        ed.WriteMessage($"\n  Groupe : {entry.Key} - {group.Count} objets");
    }
    
    tr.Commit();
}
```

### Exemple 7: Accéder au Dictionnaire de Présentations
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary layoutDict = tr.GetObject(db.LayoutDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\nPrésentations dans le dessin :");
    foreach (DBDictionaryEntry entry in layoutDict)
    {
        Layout layout = tr.GetObject(entry.Value, OpenMode.ForRead) as Layout;
        ed.WriteMessage($"\n  {layout.LayoutName}");
    }
    
    tr.Commit();
}
```

### Exemple 8: Créer une Entrée dans le Dictionnaire d'Objets Nommés
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Créer un dictionnaire personnalisé
    string dictName = "MonDictionnaireCustom";
    
    if (!nod.Contains(dictName))
    {
        DBDictionary customDict = new DBDictionary();
        nod.SetAt(dictName, customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
        
        ed.WriteMessage($"\nDictionnaire personnalisé créé : {dictName}");
    }
    
    tr.Commit();
}
```

### Travailler avec TileMode (Espace Objet/Papier)
```csharp
Database db = Application.DocumentManager.MdiActiveDocument.Database;

if (db.TileMode)
{
    // Actuellement en Espace Objet
}
else
{
    // Actuellement en Espace Papier
}
```

### Accéder à l'Espace Objet et Espace Papier
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Espace Objet
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Espace Papier
    BlockTableRecord paperSpace = tr.GetObject(bt[BlockTableRecord.PaperSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    tr.Commit();
}
```

## Objets Associés
- [Document](Document.md) - Contient la Database
- [TransactionManager](TransactionManager.md) - Gère les transactions pour la base de données
- [BlockTable](BlockTable.md) - Table de symboles pour les blocs
- [LayerTable](LayerTable.md) - Table de symboles pour les calques
- [Entity](Entity.md) - Classe de base pour les objets graphiques

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-C8C7E74A-ACAD-4E1E-B90E-2C9F6D1E7C3E)
- [Référence Classe Database](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Database)
