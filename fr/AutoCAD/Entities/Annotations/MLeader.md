# Classe MLeader

## Vue d'Ensemble
La classe `MLeader` représente une entité d'annotation multi-repère moderne dans AutoCAD (introduite dans AutoCAD 2008). Les MLeaders sont plus flexibles que les Leaders hérités, supportant plusieurs lignes de repère, divers types de contenu (texte, bloc, ou aucun), et des styles MLeader dédiés pour un formatage cohérent.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ MLeader
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `ContentType` | `ContentType` | Obtient/définit le type de contenu (MText, Block, None) |
| `MText` | `MText` | Obtient/définit le contenu MText |
| `BlockContentId` | `ObjectId` | Obtient/définit le contenu bloc |
| `MLeaderStyle` | `ObjectId` | Obtient/définit le style MLeader |
| `ArrowSymbolId` | `ObjectId` | Obtient/définit le bloc de pointe de flèche |
| `LeaderLineType` | `LeaderType` | Obtient/définit le type de ligne de repère (Straight, Spline) |
| `TextAttachmentType` | `TextAttachmentType` | Obtient/définit comment le texte s'attache au repère |
| `DoglegLength` | `double` | Obtient/définit la longueur d'atterrissage horizontal |
| `EnableDogleg` | `bool` | Obtient/définit si le dogleg est activé |
| `EnableLanding` | `bool` | Obtient/définit si la ligne d'atterrissage est activée |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `AddLeader()` | `int` | Ajoute une nouvelle ligne de repère et retourne son index |
| `RemoveLeader(int)` | `void` | Supprime le repère à l'index spécifié |
| `AddLeaderLine(int)` | `int` | Ajoute une ligne de repère à un repère existant |
| `RemoveLeaderLine(int, int)` | `void` | Supprime une ligne de repère |
| `SetFirstVertex(int, Point3d)` | `void` | Définit le premier sommet d'une ligne de repère |
| `SetLastVertex(int, Point3d)` | `void` | Définit le dernier sommet d'une ligne de repère |
| `GetLeaderIndex(int)` | `int` | Obtient l'index du repère pour une ligne de repère |
| `GetLeaderLineIndexes(int)` | `IntegerCollection` | Obtient tous les index de ligne de repère pour un repère |

## Exemples de Code

### Exemple 1: Créer un MLeader Simple avec Texte
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer MLeader
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    
    // Définir le type de contenu à MText
    mleader.ContentType = ContentType.MTextContent;
    
    // Définir le contenu texte
    MText mtext = new MText();
    mtext.Contents = "Ceci est une note MLeader";
    mleader.MText = mtext;
    
    // Ajouter une ligne de repère
    int leaderIndex = mleader.AddLeader();
    int lineIndex = mleader.AddLeaderLine(leaderIndex);
    
    // Définir les sommets (du texte vers l'élément)
    mleader.SetFirstVertex(lineIndex, new Point3d(10, 10, 0));
    mleader.SetLastVertex(lineIndex, new Point3d(0, 0, 0));
    
    // Ajouter au dessin
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Exemple 2: Créer un MLeader avec Plusieurs Lignes de Repère
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    mleader.ContentType = ContentType.MTextContent;
    
    MText mtext = new MText();
    mtext.Contents = "Repères Multiples";
    mleader.MText = mtext;
    
    // Ajouter premier repère
    int leader1 = mleader.AddLeader();
    int line1 = mleader.AddLeaderLine(leader1);
    mleader.SetFirstVertex(line1, new Point3d(10, 10, 0));
    mleader.SetLastVertex(line1, new Point3d(0, 0, 0));
    
    // Ajouter deuxième repère
    int leader2 = mleader.AddLeader();
    int line2 = mleader.AddLeaderLine(leader2);
    mleader.SetFirstVertex(line2, new Point3d(10, 10, 0));
    mleader.SetLastVertex(line2, new Point3d(5, -5, 0));
    
    // Ajouter troisième repère
    int leader3 = mleader.AddLeader();
    int line3 = mleader.AddLeaderLine(leader3);
    mleader.SetFirstVertex(line3, new Point3d(10, 10, 0));
    mleader.SetLastVertex(line3, new Point3d(-5, 5, 0));
    
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Exemple 3: Créer un MLeader avec Contenu Bloc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    
    // Définir le type de contenu à Bloc
    mleader.ContentType = ContentType.BlockContent;
    
    // Définir le contenu bloc (en supposant que le bloc "DetailTag" existe)
    if (bt.Has("DetailTag"))
    {
        mleader.BlockContentId = bt["DetailTag"];
    }
    
    // Ajouter repère
    int leaderIndex = mleader.AddLeader();
    int lineIndex = mleader.AddLeaderLine(leaderIndex);
    mleader.SetFirstVertex(lineIndex, new Point3d(10, 10, 0));
    mleader.SetLastVertex(lineIndex, new Point3d(0, 0, 0));
    
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Exemple 4: Définir les Propriétés MLeader
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForWrite) as MLeader;
    
    // Définir le type de ligne de repère
    mleader.LeaderLineType = LeaderType.SplineLeader;
    
    // Activer et définir le dogleg
    mleader.EnableDogleg = true;
    mleader.DoglegLength = 5.0;
    
    // Activer l'atterrissage
    mleader.EnableLanding = true;
    
    // Définir l'attachement texte
    mleader.TextAttachmentType = TextAttachmentType.AttachmentMiddle;
    
    // Définir la couleur
    mleader.ColorIndex = 3; // Vert
    
    tr.Commit();
}
```

### Exemple 5: Lire les Informations MLeader
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForRead) as MLeader;
    
    ed.WriteMessage("\n=== Information MLeader ===");
    ed.WriteMessage($"\nType Contenu : {mleader.ContentType}");
    ed.WriteMessage($"\nType Ligne Repère : {mleader.LeaderLineType}");
    ed.WriteMessage($"\nDogleg Activé : {mleader.EnableDogleg}");
    ed.WriteMessage($"\nLongueur Dogleg : {mleader.DoglegLength:F2}");
    ed.WriteMessage($"\nAtterrissage Activé : {mleader.EnableLanding}");
    
    if (mleader.ContentType == ContentType.MTextContent)
    {
        ed.WriteMessage($"\nContenu Texte : {mleader.MText.Contents}");
    }
    
    // Compter les repères
    int leaderCount = 0;
    for (int i = 0; i < 100; i++) // Limite supérieure arbitraire
    {
        try
        {
            IntegerCollection lines = mleader.GetLeaderLineIndexes(i);
            if (lines != null && lines.Count > 0)
                leaderCount++;
            else
                break;
        }
        catch
        {
            break;
        }
    }
    
    ed.WriteMessage($"\nNombre de Repères : {leaderCount}");
    
    tr.Commit();
}
```

### Exemple 6: Appliquer un Style MLeader
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForWrite) as MLeader;
    
    // Obtenir le dictionnaire de style MLeader
    DBDictionary mleaderDict = tr.GetObject(db.MLeaderStyleDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    if (mleaderDict.Contains("Annotative"))
    {
        mleader.MLeaderStyle = mleaderDict.GetAt("Annotative");
        ed.WriteMessage("\nStyle MLeader 'Annotative' appliqué");
    }
    
    tr.Commit();
}
```

### Exemple 7: Créer un MLeader sans Contenu
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer MLeader sans contenu (juste lignes de repère)
    MLeader mleader = new MLeader();
    mleader.SetDatabaseDefaults();
    mleader.ContentType = ContentType.NoneContent;
    
    // Ajouter repère
    int leaderIndex = mleader.AddLeader();
    int lineIndex = mleader.AddLeaderLine(leaderIndex);
    mleader.SetFirstVertex(lineIndex, new Point3d(10, 0, 0));
    mleader.SetLastVertex(lineIndex, new Point3d(0, 0, 0));
    
    mleader.ColorIndex = 1; // Rouge
    
    btr.AppendEntity(mleader);
    tr.AddNewlyCreatedDBObject(mleader, true);
    
    tr.Commit();
}
```

### Exemple 8: Modifier le Texte MLeader
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    MLeader mleader = tr.GetObject(mleaderId, OpenMode.ForWrite) as MLeader;
    
    if (mleader.ContentType == ContentType.MTextContent)
    {
        MText mtext = mleader.MText;
        mtext.Contents = "Contenu texte mis à jour\\PAvec plusieurs lignes";
        mtext.TextHeight = 3.0;
        mleader.MText = mtext;
        
        ed.WriteMessage("\nTexte MLeader mis à jour");
    }
    
    tr.Commit();
}
```

## Types de Contenu

| Type | Description |
|------|-------------|
| `MTextContent` | Annotation texte multi-lignes |
| `BlockContent` | Annotation référence de bloc |
| `NoneContent` | Aucun contenu (lignes de repère seulement) |

## Types de Repère

| Type | Description |
|------|-------------|
| `StraightLeader` | Segments de ligne droits |
| `SplineLeader` | Courbe spline lisse |

## Types d'Attachement Texte

| Type | Description |
|------|-------------|
| `AttachmentTop` | Attacher au haut du texte |
| `AttachmentMiddle` | Attacher au milieu du texte |
| `AttachmentBottom` | Attacher au bas du texte |

## MLeader vs Leader

| Fonctionnalité | Leader (Hérité) | MLeader (Moderne) |
|----------------|-----------------|-------------------|
| Introduction | R13 | 2008 |
| Repères Multiples | Non | Oui |
| Types de Contenu | Limité | Texte, Bloc, Aucun |
| Styles | Style de Dimension | Style MLeader |
| Flexibilité | Basique | Avancée |
| Recommandé | Hérité seulement | Tout nouveau travail |

## Meilleures Pratiques

1. **Utiliser Styles MLeader** : Créez et utilisez des styles MLeader pour la cohérence
2. **Type de Contenu** : Choisissez le type de contenu approprié pour vos besoins
3. **Repères Multiples** : Utilisez pour pointer vers plusieurs éléments
4. **Dogleg** : Activez pour une ligne d'atterrissage horizontale
5. **Repères Spline** : Utilisez pour une apparence organique et courbée
6. **Formatage Texte** : Utilisez les codes de formatage MText pour le texte riche
7. **Contenu Bloc** : Utilisez pour les étiquettes et appels standardisés

## Objets Associés
- [Leader](Leader.md) - Annotation repère héritée
- [Dimension](Dimension.md) - Classe de base d'annotation de dimension
- [Entity](../../BaseClasses/Entity.md) - Classe de base pour toutes les entités
- MText - Contenu texte pour MLeaders
- BlockReference - Contenu bloc pour MLeaders
- MLeaderStyle - Définition de style pour MLeaders

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence Classe MLeader](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_MLeader)
