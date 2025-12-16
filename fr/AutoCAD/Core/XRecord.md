# Classe XRecord

## Vue d'Ensemble
La classe `XRecord` est un objet de stockage de données personnalisé qui peut être stocké dans des dictionnaires (tels que le Dictionnaire des Objets Nommés ou les dictionnaires d'extension). Contrairement aux XData qui sont limitées à ~16KB et attachées directement aux entités, les XRecords peuvent stocker des quantités illimitées de données structurées et sont stockés dans des dictionnaires en tant qu'entrées nommées.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ XRecord
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Data` | `ResultBuffer` | Obtient/définit les données stockées dans le XRecord |
| `IsClonable` | `bool` | Obtient si le XRecord peut être cloné |
| `MergeStyle` | `DictionaryMergeStyle` | Obtient/définit comment le XRecord est fusionné durant WBLOCK |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `XRecord()` | Constructeur | Crée un nouveau XRecord vide |
| `Dispose()` | `void` | Libère le XRecord et les ressources |

## Exemples de Code

### Exemple 1: Créer un XRecord dans le Dictionnaire des Objets Nommés
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir le Dictionnaire des Objets Nommés (NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Créer un dictionnaire personnalisé s'il n'existe pas
    string dictName = "MyAppData";
    DBDictionary customDict;
    
    if (nod.Contains(dictName))
    {
        customDict = tr.GetObject(nod.GetAt(dictName), OpenMode.ForWrite) as DBDictionary;
    }
    else
    {
        customDict = new DBDictionary();
        nod.SetAt(dictName, customDict);
        tr.AddNewlyCreatedDBObject(customDict, true);
    }
    
    // Créer un XRecord avec des données
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "Nom Projet"),
        new TypedValue((int)DxfCode.Int32, 12345),
        new TypedValue((int)DxfCode.Real, 3.14159)
    );
    xRec.Data = rb;
    
    // Ajouter XRecord au dictionnaire personnalisé
    customDict.SetAt("ProjectInfo", xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    tr.Commit();
}
```

### Exemple 2: Lire des Données XRecord
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyAppData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyAppData"), OpenMode.ForRead) as DBDictionary;
        
        if (customDict.Contains("ProjectInfo"))
        {
            XRecord xRec = tr.GetObject(customDict.GetAt("ProjectInfo"), OpenMode.ForRead) as XRecord;
            
            ResultBuffer rb = xRec.Data;
            
            if (rb != null)
            {
                foreach (TypedValue tv in rb)
                {
                    ed.WriteMessage($"\nCode : {tv.TypeCode}, Valeur : {tv.Value}");
                }
                
                rb.Dispose();
            }
        }
    }
    
    tr.Commit();
}
```

### Exemple 3: Stocker des Données Structurées Complexes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Créer XRecord avec données structurées
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        // En-tête
        new TypedValue((int)DxfCode.Text, "HEADER"),
        new TypedValue((int)DxfCode.Text, "Version"),
        new TypedValue((int)DxfCode.Text, "1.0"),
        
        // Info projet
        new TypedValue((int)DxfCode.Text, "PROJECT"),
        new TypedValue((int)DxfCode.Text, "Nom"),
        new TypedValue((int)DxfCode.Text, "Bâtiment A"),
        new TypedValue((int)DxfCode.Text, "Date"),
        new TypedValue((int)DxfCode.Text, DateTime.Now.ToString("yyyy-MM-dd")),
        
        // Données numériques
        new TypedValue((int)DxfCode.Text, "METRICS"),
        new TypedValue((int)DxfCode.Real, 1234.56),
        new TypedValue((int)DxfCode.Real, 7890.12),
        
        // Point 3D
        new TypedValue((int)DxfCode.Point3d, new Point3d(100, 200, 0))
    );
    xRec.Data = rb;
    
    if (!nod.Contains("AppSettings"))
    {
        DBDictionary appDict = new DBDictionary();
        nod.SetAt("AppSettings", appDict);
        tr.AddNewlyCreatedDBObject(appDict, true);
        
        appDict.SetAt("Config", xRec);
        tr.AddNewlyCreatedDBObject(xRec, true);
    }
    
    rb.Dispose();
    
    tr.Commit();
}
```

### Exemple 4: Attacher XRecord au Dictionnaire d'Extension d'Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Obtenir une entité (ex: une ligne)
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Créer ou obtenir le dictionnaire d'extension de l'entité
    if (ent.ExtensionDictionary == ObjectId.Null)
    {
        ent.CreateExtensionDictionary();
    }
    
    DBDictionary extDict = tr.GetObject(ent.ExtensionDictionary, OpenMode.ForWrite) as DBDictionary;
    
    // Créer XRecord avec données personnalisées
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "CustomProperty"),
        new TypedValue((int)DxfCode.Text, "CustomValue"),
        new TypedValue((int)DxfCode.Int32, 42)
    );
    xRec.Data = rb;
    
    // Ajouter au dictionnaire d'extension
    string key = "MyAppData";
    if (extDict.Contains(key))
    {
        extDict.Remove(key);
    }
    
    extDict.SetAt(key, xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    ed.WriteMessage($"\nXRecord attaché à l'entité {ent.GetType().Name}");
    
    tr.Commit();
}
```

### Exemple 5: Mettre à Jour les Données XRecord
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyAppData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyAppData"), OpenMode.ForRead) as DBDictionary;
        
        if (customDict.Contains("ProjectInfo"))
        {
            XRecord xRec = tr.GetObject(customDict.GetAt("ProjectInfo"), OpenMode.ForWrite) as XRecord;
            
            // Mettre à jour les données
            ResultBuffer newRb = new ResultBuffer(
                new TypedValue((int)DxfCode.Text, "Nom Projet Mis à Jour"),
                new TypedValue((int)DxfCode.Int32, 99999),
                new TypedValue((int)DxfCode.Real, 2.71828),
                new TypedValue((int)DxfCode.Text, "Nouveau Champ")
            );
            
            xRec.Data = newRb;
            newRb.Dispose();
            
            ed.WriteMessage("\nDonnées XRecord mises à jour avec succès");
        }
    }
    
    tr.Commit();
}
```

### Exemple 6: Supprimer un XRecord
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (nod.Contains("MyAppData"))
    {
        DBDictionary customDict = tr.GetObject(nod.GetAt("MyAppData"), OpenMode.ForWrite) as DBDictionary;
        
        if (customDict.Contains("ProjectInfo"))
        {
            // Supprimer le XRecord du dictionnaire
            customDict.Remove("ProjectInfo");
            
            ed.WriteMessage("\nXRecord supprimé avec succès");
        }
    }
    
    tr.Commit();
}
```

### Exemple 7: Recherche de XRecords dans le Dictionnaire des Objets Nommés
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    ed.WriteMessage("\n=== Recherche XRecords dans NOD ===");
    
    foreach (DBDictionaryEntry entry in nod)
    {
        DBObject obj = tr.GetObject(entry.Value, OpenMode.ForRead);
        
        if (obj is XRecord)
        {
            XRecord xRec = obj as XRecord;
            ed.WriteMessage($"\nTrouvé XRecord : {entry.Key}");
            
            ResultBuffer rb = xRec.Data;
            if (rb != null)
            {
                ed.WriteMessage($"  Éléments de données : {rb.AsArray().Length}");
                rb.Dispose();
            }
        }
        else if (obj is DBDictionary)
        {
            // Recherche récursive dictionnaires imbriqués
            DBDictionary subDict = obj as DBDictionary;
            ed.WriteMessage($"\nRecherche dictionnaire : {entry.Key}");
            
            foreach (DBDictionaryEntry subEntry in subDict)
            {
                DBObject subObj = tr.GetObject(subEntry.Value, OpenMode.ForRead);
                if (subObj is XRecord)
                {
                    ed.WriteMessage($"  Trouvé XRecord : {subEntry.Key}");
                }
            }
        }
    }
    
    tr.Commit();
}
```

### Exemple 8: Stocker des Données Binaires dans XRecord
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForWrite) as DBDictionary;
    
    // Créer XRecord avec données binaires
    XRecord xRec = new XRecord();
    
    // Exemple : Stocker un objet sérialisé comme données binaires
    byte[] binaryData = new byte[] { 0x01, 0x02, 0x03, 0x04, 0xFF, 0xFE };
    
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "BinaryDataRecord"),
        new TypedValue((int)DxfCode.BinaryChunk, binaryData),
        new TypedValue((int)DxfCode.Int32, binaryData.Length)
    );
    xRec.Data = rb;
    
    if (!nod.Contains("BinaryData"))
    {
        DBDictionary binaryDict = new DBDictionary();
        nod.SetAt("BinaryData", binaryDict);
        tr.AddNewlyCreatedDBObject(binaryDict, true);
        
        binaryDict.SetAt("Data1", xRec);
        tr.AddNewlyCreatedDBObject(xRec, true);
    }
    
    rb.Dispose();
    
    ed.WriteMessage($"\nStocké {binaryData.Length} octets dans XRecord");
    
    tr.Commit();
}
```

## Codes DXF Communs pour Données XRecord

| Code | Type | Description |
|------|------|-------------|
| `1` | String | Chaîne de texte |
| `10` | Point3d | Point 3D |
| `40` | Double | Valeur à virgule flottante |
| `70` | Int16 | Entier 16-bit |
| `90` | Int32 | Entier 32-bit |
| `280` | Byte | Entier 8-bit |
| `310` | Binary | Morceau de données binaires |
| `330` | ObjectId | ID pointeur-soft/handle |
| `340` | ObjectId | ID pointeur-hard/handle |
| `360` | ObjectId | ID propriétaire-hard/handle |

## Meilleures Pratiques

1. **Utiliser des Dictionnaires pour l'Organisation** : Stockez les XRecords dans des dictionnaires personnalisés dans le Dictionnaire des Objets Nommés pour une meilleure organisation
2. **Libérer les ResultBuffers** : Libérez toujours les objets ResultBuffer pour éviter les fuites de mémoire
3. **Utiliser des Clés Significatives** : Utilisez des clés de dictionnaire descriptives pour les XRecords
4. **Structurer Vos Données** : Planifiez votre structure de données en utilisant des codes DXF cohérents
5. **Dictionnaires d'Extension** : Utilisez les dictionnaires d'extension d'entité pour attacher des XRecords à des entités spécifiques
6. **Gestion de Transaction** : Utilisez toujours des transactions lors de la création ou modification de XRecords
7. **Vérifier l'Existence** : Vérifiez si les entrées de dictionnaire existent avant d'y accéder
8. **Documenter Votre Schéma** : Documentez la structure et les codes DXF utilisés dans vos XRecords

## XRecord vs XData

| Fonctionnalité | XRecord | XData |
|----------------|---------|-------|
| Emplacement Stockage | Dictionnaires (NOD, Dict Extension) | Attaché directement aux entités |
| Limite de Taille | Illimité | ~16KB par entité |
| Structure | Stocké comme entrées nommées | Identifié par nom d'application |
| Organisation | Hiérarchique (dictionnaires imbriqués) | Plat (par entité) |
| Accessibilité | Nécessite navigation dictionnaire | Propriété d'entité directe |
| Cas d'Usage | Données larges/complexes, données partagées | Petites données spécifiques à l'entité |
| Performance | Légèrement plus lent (recherche dict) | Plus rapide (accès direct) |

## Objets Associés
- [DBDictionary](DBDictionary.md) - Conteneur pour XRecords
- [Database](Database.md) - Fournit accès au Dictionnaire des Objets Nommés
- [XData](../BaseClasses/XData.md) - Alternative pour petites données spécifiques à l'entité
- [Entity](../BaseClasses/Entity.md) - Peut avoir des dictionnaires d'extension contenant des XRecords
- ResultBuffer - Conteneur de données pour contenu XRecord
- TypedValue - Valeurs de données individuelles dans ResultBuffer

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe XRecord](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Xrecord)
- [Travailler avec les Dictionnaires](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-A809CD71-4655-44E2-B674-1FE200B9FE75)
