# XData (Données d'Entité Étendues)

## Vue d'Ensemble
Les XData (Extended Entity Data) vous permettent d'attacher des données spécifiques à une application personnalisée aux entités AutoCAD. Ces données persistent avec le dessin et peuvent être utilisées pour stocker des informations pour des applications personnalisées.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Concepts Clés

### Enregistrement d'Application
Avant d'attacher des XData, vous devez enregistrer votre nom d'application dans la RegAppTable.

### ResultBuffer
Les XData sont stockées et récupérées en utilisant des objets `ResultBuffer`, qui contiennent des valeurs typées.

### Codes DXF
Les XData utilisent des codes DXF (Drawing Interchange Format) pour identifier les types de données :
- `1001` - Nom d'application (première entrée requise)
- `1000` - Chaîne
- `1040` - Double
- `1070` - Entier 16-bit
- `1071` - Entier 32-bit
- `1010` - Point 3D
- `1011` - Déplacement 3D
- `1012` - Direction 3D
- `1013` - Distance 3D

## Exemples de Code

### Exemple 1: Enregistrer une Application
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    string appName = "MonApp";
    
    if (!rat.Has(appName))
    {
        rat.UpgradeOpen();
        
        RegAppTableRecord ratr = new RegAppTableRecord();
        ratr.Name = appName;
        
        rat.Add(ratr);
        tr.AddNewlyCreatedDBObject(ratr, true);
        
        ed.WriteMessage($"\nApplication enregistrée : {appName}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Attacher des XData à une Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Créer XData
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MonApp"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Données Personnalisées"),
        new TypedValue((int)DxfCode.ExtendedDataReal, 123.45),
        new TypedValue((int)DxfCode.ExtendedDataInteger32, 100)
    );
    
    // Attacher XData à l'entité
    ent.XData = rb;
    
    rb.Dispose();
    
    tr.Commit();
}
```

### Exemple 3: Lire des XData depuis une Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Obtenir tout les XData
    ResultBuffer rb = ent.XData;
    
    if (rb != null)
    {
        foreach (TypedValue tv in rb)
        {
            ed.WriteMessage($"\nCode : {tv.TypeCode}, Valeur : {tv.Value}");
        }
        
        rb.Dispose();
    }
    else
    {
        ed.WriteMessage("\nAucun XData attaché");
    }
    
    tr.Commit();
}
```

### Exemple 4: Obtenir des XData pour une Application Spécifique
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Obtenir XData pour application spécifique
    ResultBuffer rb = ent.GetXDataForApplication("MonApp");
    
    if (rb != null)
    {
        ed.WriteMessage("\nXData pour 'MonApp' :");
        
        foreach (TypedValue tv in rb)
        {
            ed.WriteMessage($"\n  Code : {tv.TypeCode}, Valeur : {tv.Value}");
        }
        
        rb.Dispose();
    }
    
    tr.Commit();
}
```

### Exemple 5: Mettre à Jour des XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Obtenir XData existants
    ResultBuffer existingRb = ent.GetXDataForApplication("MonApp");
    
    // Créer nouveaux XData (remplace existant pour cette app)
    ResultBuffer newRb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MonApp"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Données Mises à Jour"),
        new TypedValue((int)DxfCode.ExtendedDataReal, 456.78)
    );
    
    ent.XData = newRb;
    
    if (existingRb != null) existingRb.Dispose();
    newRb.Dispose();
    
    tr.Commit();
}
```

### Exemple 6: Supprimer des XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Pour supprimer les XData pour une application spécifique, définissez-les à null via un buffer minimal
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MonApp")
    );
    
    ent.XData = rb;
    rb.Dispose();
    
    tr.Commit();
}
```

### Exemple 7: Stocker des Données Complexes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Stocker divers types de données
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MonApp"),
        
        // Chaîne
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Nom Projet"),
        
        // Entier
        new TypedValue((int)DxfCode.ExtendedDataInteger32, 12345),
        
        // Double
        new TypedValue((int)DxfCode.ExtendedDataReal, 3.14159),
        
        // Point 3D
        new TypedValue((int)DxfCode.ExtendedDataWorldSpacePosition, new Point3d(100, 200, 0)),
        
        // Valeurs multiples
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Statut"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Approuvé")
    );
    
    ent.XData = rb;
    rb.Dispose();
    
    tr.Commit();
}
```

## Codes DXF Communs pour XData

| Code | Type | Description |
|------|------|-------------|
| `1001` | String | Nom d'application (requis) |
| `1000` | String | Chaîne ASCII |
| `1002` | String | Chaîne de contrôle ("{" ou "}") |
| `1003` | String | Nom de calque |
| `1004` | Binary | Données binaires |
| `1005` | String | Handle de base de données |
| `1010` | Point3d | Position espace monde |
| `1011` | Point3d | Déplacement espace monde |
| `1012` | Point3d | Direction espace monde |
| `1013` | Point3d | Distance monde |
| `1040` | Double | Nombre réel |
| `1041` | Double | Distance |
| `1042` | Double | Facteur d'échelle |
| `1070` | Int16 | Entier 16-bit |
| `1071` | Int32 | Entier 32-bit |

## Meilleures Pratiques

1. **Toujours Enregistrer l'Application** : Enregistrez votre nom d'application avant d'attacher des XData
2. **Libérer ResultBuffers** : Disposez toujours les objets ResultBuffer pour éviter les fuites de mémoire
3. **Vérifier Null** : Vérifiez toujours si des XData existent avant de traiter
4. **Utiliser des Noms Significatifs** : Utilisez des noms d'application descriptifs
5. **Documenter la Structure** : Documentez la structure de vos XData pour référence future
6. **Limites de Taille** : Les XData ont des limites de taille (environ 16KB par entité)

## XData vs Dictionnaire d'Extension

| Fonctionnalité | XData | Dictionnaire d'Extension |
|----------------|-------|--------------------------|
| Stockage | Limité (~16KB) | Illimité |
| Structure | Liste plate | Hiérarchique |
| Complexité | Simple | Objets complexes |
| Performance | Rapide | Plus lent |

## Objets Associés
- [Entity](Entity.md) - Classe de base avec méthodes XData
- [RegAppTable](../SymbolTables/RegAppTable.md) - Enregistrement d'application
- ResultBuffer - Conteneur XData
- TypedValue - Valeurs XData individuelles

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
