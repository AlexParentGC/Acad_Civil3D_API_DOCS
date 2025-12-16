# API .NET AutoCAD et Civil3D - Workflow d'Acquisition d'Objets

Ce document fournit un flux de travail complet pour accéder à tous les types d'objets dans l'arborescence du modèle d'objet de l'API .NET AutoCAD et Civil3D en utilisant C#.

## Table des Matières
- [Workflow API AutoCAD](#workflow-api-autocad)
- [Workflow API Civil3D](#workflow-api-civil3d)
- [Modèles Communs](#modèles-communs)
- [Meilleures Pratiques](#meilleures-pratiques)

---

## Workflow API AutoCAD

### 1. Accéder à l'Application

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.EditorInput;

// Obtenir l'application AutoCAD actuelle
Application acApp = Application.DocumentManager.MdiActiveDocument.Application;
```

### 2. Accéder aux Documents

```csharp
// Obtenir le gestionnaire de documents
DocumentCollection docMgr = Application.DocumentManager;

// Obtenir le document actif
Document acDoc = docMgr.MdiActiveDocument;

// Ou itérer à travers tous les documents ouverts
foreach (Document doc in docMgr)
{
    // Travailler avec chaque document
}
```

### 3. Accéder à la Base de Données

```csharp
// Obtenir la base de données du document actif
Database db = acDoc.Database;

// Ou obtenir la base de données de travail
Database workingDb = HostApplicationServices.WorkingDatabase;
```

### 4. Accéder aux Tables de Symboles

Tout accès aux tables de symboles suit un modèle similaire utilisant des transactions :

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // BlockTable (Table des blocs)
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // LayerTable (Table des calques)
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    // LinetypeTable (Table des types de ligne)
    LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
    
    // TextStyleTable (Table des styles de texte)
    TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForRead) as TextStyleTable;
    
    // DimStyleTable (Table des styles de cote)
    DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
    
    // UcsTable (Table des SCU)
    UcsTable ut = tr.GetObject(db.UcsTableId, OpenMode.ForRead) as UcsTable;
    
    // ViewTable (Table des vues)
    ViewTable vt = tr.GetObject(db.ViewTableId, OpenMode.ForRead) as ViewTable;
    
    // ViewportTable (Table des fenêtres)
    ViewportTable vpt = tr.GetObject(db.ViewportTableId, OpenMode.ForRead) as ViewportTable;
    
    // RegAppTable (Applications Enregistrées)
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    tr.Commit();
}
```

### 5. Accéder aux BlockTableRecords et aux Entités

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    
    // Accéder à l'Espace Objet (Model Space)
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Accéder à l'Espace Papier (Paper Space)
    BlockTableRecord paperSpace = tr.GetObject(bt[BlockTableRecord.PaperSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    // Itérer à travers les entités de l'Espace Objet
    foreach (ObjectId objId in modelSpace)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        // Vérifier le type d'entité et caster de manière appropriée
        if (ent is Line line)
        {
            // Travailler avec une ligne
        }
        else if (ent is Arc arc)
        {
            // Travailler avec un arc
        }
        else if (ent is Circle circle)
        {
            // Travailler avec un cercle
        }
        else if (ent is Polyline pline)
        {
            // Travailler avec une polyligne légère
        }
        else if (ent is Polyline2d pline2d)
        {
            // Travailler avec une polyligne 2D
        }
        else if (ent is Polyline3d pline3d)
        {
            // Travailler avec une polyligne 3D
        }
        else if (ent is Ellipse ellipse)
        {
            // Travailler avec une ellipse
        }
        else if (ent is Spline spline)
        {
            // Travailler avec une spline
        }
        else if (ent is BlockReference blockRef)
        {
            // Travailler avec une référence de bloc
        }
        else if (ent is DBText dbText)
        {
            // Travailler avec un texte sur une ligne
        }
        else if (ent is MText mText)
        {
            // Travailler avec un texte multi-lignes
        }
        else if (ent is Dimension dim)
        {
            // Travailler avec une cote
        }
        else if (ent is Hatch hatch)
        {
            // Travailler avec des hachures
        }
        else if (ent is Leader leader)
        {
            // Travailler avec une ligne de repère
        }
        else if (ent is MLeader mLeader)
        {
            // Travailler avec un multi-repère
        }
        else if (ent is Solid3d solid)
        {
            // Travailler avec un solide 3D
        }
        else if (ent is Region region)
        {
            // Travailler avec une région
        }
        else if (ent is Body body)
        {
            // Travailler avec un corps
        }
        else if (ent is SubDMesh mesh)
        {
            // Travailler avec un maillage de subdivision
        }
        else if (ent is Viewport viewport)
        {
            // Travailler avec une fenêtre
        }
    }
    
    tr.Commit();
}
```

### 6. Travailler avec les Objets Courbe (Curve)

Toutes les entités basées sur des courbes héritent de la classe `Curve` :

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    foreach (ObjectId objId in btr)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        if (ent is Curve curve)
        {
            // Propriétés et méthodes communes aux courbes
            double length = curve.GetDistanceAtParameter(curve.EndParam);
            Point3d startPoint = curve.StartPoint;
            Point3d endPoint = curve.EndPoint;
            
            // Obtenir un point à un paramètre donné
            Point3d midPoint = curve.GetPointAtParameter(
                (curve.StartParam + curve.EndParam) / 2);
        }
    }
    
    tr.Commit();
}
```

### 7. Accéder aux Composants de l'Interface Utilisateur (UI)

```csharp
using Autodesk.Windows; // Pour le Ruban (Ribbon)
using Autodesk.AutoCAD.Windows; // Pour PaletteSet

// Accéder au Ruban
RibbonControl ribbon = ComponentManager.Ribbon;
if (ribbon != null)
{
    // Travailler avec les onglets et panneaux du ruban
}

// Créer un PaletteSet
PaletteSet ps = new PaletteSet("Ma Palette");
ps.Add("Mon Contrôle", new System.Windows.Forms.UserControl());
ps.Visible = true;

// Accéder à l'Éditeur
Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
```

### 8. Créer des Éléments de Ruban

```csharp
using Autodesk.Windows;
using System.Windows.Input;

public void CreateRibbon()
{
    RibbonControl ribbon = ComponentManager.Ribbon;
    if (ribbon == null) return;

    // 1. Créer un Onglet
    RibbonTab tab = new RibbonTab();
    tab.Title = "Mon Onglet Personnalisé";
    tab.Id = "MY_CUSTOM_TAB_ID";
    ribbon.Tabs.Add(tab);

    // 2. Créer un Panneau
    RibbonPanelSource panelSource = new RibbonPanelSource();
    panelSource.Title = "Mon Panneau";
    RibbonPanel panel = new RibbonPanel();
    panel.Source = panelSource;
    tab.Panels.Add(panel);

    // 3. Créer un Bouton
    RibbonButton button = new RibbonButton();
    button.Text = "Mon Bouton";
    button.ShowText = true;
    button.ShowImage = true;
    // button.Image = ... (Charger BitmapImage)
    button.Size = RibbonItemSize.Large;
    button.Orientation = System.Windows.Controls.Orientation.Vertical;

    // 4. Assigner un Gestionnaire de Commande
    button.CommandHandler = new RibbonCommandHandler();
    button.CommandParameter = "MY_COMMAND "; // Espace à la fin pour exécuter

    panelSource.Items.Add(button);
    
    // Définir l'onglet comme actif (optionnel)
    tab.IsActive = true;
}

// Implémentation du Gestionnaire de Commande
public class RibbonCommandHandler : System.Windows.Input.ICommand
{
    public bool CanExecute(object parameter) => true;
    
    public event EventHandler CanExecuteChanged;
    
    public void Execute(object parameter)
    {
        if (parameter is string cmd)
        {
            // Envoyer la commande à AutoCAD
            Autodesk.AutoCAD.ApplicationServices.Application.DocumentManager.MdiActiveDocument
                .SendStringToExecute(cmd, true, false, true);
        }
    }
}
```

### 9. Travailler avec les Objets de Base de Données

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Accéder au Dictionnaire d'Objets Nommés (Named Objects Dictionary - NOD)
    DBDictionary nod = tr.GetObject(db.NamedObjectsDictionaryId, OpenMode.ForRead) as DBDictionary;
    
    // Vérifier la présence d'un dictionnaire spécifique
    if (nod.Contains("ACAD_GROUP"))
    {
        ObjectId groupId = nod.GetAt("ACAD_GROUP");
        DBDictionary groupDict = tr.GetObject(groupId, OpenMode.ForRead) as DBDictionary;
    }
    
    // Accéder aux XRecords (par exemple, dans le dictionnaire d'extension)
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    if (ent.ExtensionDictionary != ObjectId.Null)
    {
        DBDictionary extDict = tr.GetObject(ent.ExtensionDictionary, OpenMode.ForRead) as DBDictionary;
        if (extDict.Contains("MyData"))
        {
            XRecord xRec = tr.GetObject(extDict.GetAt("MyData"), OpenMode.ForRead) as XRecord;
            foreach (TypedValue tv in xRec.Data)
            {
                // Traiter les données
            }
        }
    }
    
    tr.Commit();
}
```

### 10. Travailler avec XData

Les Données Étendues (Extended Entity Data - XData) vous permettent d'attacher des données d'application personnalisées aux entités.

#### 1. Enregistrer une Application

Avant d'attacher des XData, vous devez enregistrer le nom de votre application dans la `RegAppTable`.

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    string appName = "MyApp";
    
    if (!rat.Has(appName))
    {
        rat.UpgradeOpen();
        RegAppTableRecord ratr = new RegAppTableRecord();
        ratr.Name = appName;
        rat.Add(ratr);
        tr.AddNewlyCreatedDBObject(ratr, true);
    }
    
    tr.Commit();
}
```

#### 2. Attacher des XData

Utilisez `ResultBuffer` et `DxfCode` pour stocker les données. La première valeur **doit** être le nom de l'application enregistrée (DxfCode 1001).

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MyApp"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Valeur Personnalisée"),
        new TypedValue((int)DxfCode.ExtendedDataReal, 123.45)
    );
    
    ent.XData = rb;
    rb.Dispose();
    
    tr.Commit();
}
```

#### 3. Lire des XData

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Obtenir les XData spécifiquement pour votre application
    ResultBuffer rb = ent.GetXDataForApplication("MyApp");
    
    if (rb != null)
    {
        foreach (TypedValue tv in rb)
        {
            // Traiter les données selon le TypeCode
            if (tv.TypeCode == (int)DxfCode.ExtendedDataAsciiString)
            {
                string value = (string)tv.Value;
            }
        }
        rb.Dispose();
    }
    
    tr.Commit();
}
```

---

## Workflow API Civil3D

### 1. Accéder à CivilApplication

```csharp
using Autodesk.Civil.ApplicationServices;
using Autodesk.Civil.DatabaseServices;

// Obtenir l'application Civil
CivilApplication civilApp = CivilApplication.ActiveDocument.Application;
```

### 2. Accéder à CivilDocument

```csharp
// Obtenir le document Civil actif
CivilDocument civilDoc = CivilApplication.ActiveDocument;

// Ou depuis un document AutoCAD
Document acDoc = Application.DocumentManager.MdiActiveDocument;
CivilDocument civilDoc = CivilDocument.GetCivilDocument(acDoc.Database);
```

### 3. Accéder aux Collections d'Objets Civil3D

Tous les objets Civil3D sont accessibles via `ObjectIdCollection` :

```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    // Alignments (Alignements)
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    foreach (ObjectId alignId in alignmentIds)
    {
        Alignment alignment = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
        // Travailler avec l'alignement
    }

    // Sites et Parcelles
    ObjectIdCollection siteIds = civilDoc.GetSiteIds();
    foreach (ObjectId siteId in siteIds)
    {
        Site site = tr.GetObject(siteId, OpenMode.ForRead) as Site;
        
        // Obtenir les parcelles dans ce site
        ObjectIdCollection parcelIds = site.GetParcelIds();
        foreach (ObjectId parcelId in parcelIds)
        {
            Parcel parcel = tr.GetObject(parcelId, OpenMode.ForRead) as Parcel;
            // Travailler avec la parcelle
        }
        
        // Utiliser site.GetAlignmentIds() pour les alignements spécifiques au site
    }
    
    // Surfaces
    ObjectIdCollection surfaceIds = civilDoc.GetSurfaceIds();
    foreach (ObjectId surfId in surfaceIds)
    {
        Surface surface = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
        
        // Vérifier le type de surface
        if (surface is TinSurface tinSurf)
        {
            // Travailler avec une surface TIN
        }
        else if (surface is GridSurface gridSurf)
        {
            // Travailler avec une surface Grille
        }
        else if (surface is TinVolumeSurface tinVolSurf)
        {
            // Travailler avec une surface volumique TIN
        }
        else if (surface is GridVolumeSurface gridVolSurf)
        {
            // Travailler avec une surface volumique Grille
        }
    }
    
    // Points COGO
    ObjectIdCollection cogoPointIds = civilDoc.CogoPoints.GetPointIds();
    foreach (ObjectId pointId in cogoPointIds)
    {
        CogoPoint cogoPoint = tr.GetObject(pointId, OpenMode.ForRead) as CogoPoint;
        // Travailler avec le point COGO
    }
    
    // Pipe Networks (Réseaux de canalisations)
    ObjectIdCollection networkIds = civilDoc.GetPipeNetworkIds();
    foreach (ObjectId networkId in networkIds)
    {
        Network network = tr.GetObject(networkId, OpenMode.ForRead) as Network;
        
        // Obtenir les tuyaux du réseau
        ObjectIdCollection pipeIds = network.GetPipeIds();
        foreach (ObjectId pipeId in pipeIds)
        {
            Pipe pipe = tr.GetObject(pipeId, OpenMode.ForRead) as Pipe;
            // Travailler avec le tuyau
        }
        
        // Obtenir les structures du réseau
        ObjectIdCollection structureIds = network.GetStructureIds();
        foreach (ObjectId structId in structureIds)
        {
            Structure structure = tr.GetObject(structId, OpenMode.ForRead) as Structure;
            // Travailler avec la structure
        }
    }
    
    // Corridors (Projets routiers)
    ObjectIdCollection corridorIds = civilDoc.GetCorridorIds();
    foreach (ObjectId corridorId in corridorIds)
    {
        Corridor corridor = tr.GetObject(corridorId, OpenMode.ForRead) as Corridor;
        // Travailler avec le corridor
    }
    
    // Assemblies (Profils types)
    ObjectIdCollection assemblyIds = civilDoc.GetAssemblyIds();
    foreach (ObjectId assemblyId in assemblyIds)
    {
        Assembly assembly = tr.GetObject(assemblyId, OpenMode.ForRead) as Assembly;
        // Travailler avec l'assemblage
    }
    
    // Catchments (Bassins versants)
    ObjectIdCollection catchmentIds = civilDoc.GetCatchmentIds();
    foreach (ObjectId catchmentId in catchmentIds)
    {
        Catchment catchment = tr.GetObject(catchmentId, OpenMode.ForRead) as Catchment;
        // Travailler avec le bassin versant
    }
    
    // Gradings (Nivrelements)
    ObjectIdCollection gradingIds = civilDoc.GetGradingIds();
    foreach (ObjectId gradingId in gradingIds)
    {
        Grading grading = tr.GetObject(gradingId, OpenMode.ForRead) as Grading;
        // Travailler avec le nivellement
    }
    
    // Feature Lines (Lignes caractéristiques)
    ObjectIdCollection featureLineIds = civilDoc.GetFeatureLineIds();
    foreach (ObjectId featureLineId in featureLineIds)
    {
        FeatureLine featureLine = tr.GetObject(featureLineId, OpenMode.ForRead) as FeatureLine;
        // Travailler avec la ligne caractéristique
    }
    
    // Sample Lines (Lignes de profil en travers)
    ObjectIdCollection sampleLineIds = civilDoc.GetSampleLineGroupIds();
    foreach (ObjectId sampleLineGroupId in sampleLineIds)
    {
        SampleLineGroup sampleLineGroup = tr.GetObject(sampleLineGroupId, 
            OpenMode.ForRead) as SampleLineGroup;
        
        ObjectIdCollection sampleLineIds2 = sampleLineGroup.GetSampleLineIds();
        foreach (ObjectId sampleLineId in sampleLineIds2)
        {
            SampleLine sampleLine = tr.GetObject(sampleLineId, OpenMode.ForRead) as SampleLine;
            // Travailler avec la ligne de profil en travers
        }
    }
    
    tr.Commit();
}
```

### 4. Accéder aux Profils (Associés aux Alignements)

```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    
    foreach (ObjectId alignId in alignmentIds)
    {
        Alignment alignment = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
        
        // Obtenir les vues de profil
        ObjectIdCollection profileViewIds = alignment.GetProfileViewIds();
        
        foreach (ObjectId pvId in profileViewIds)
        {
            ProfileView profileView = tr.GetObject(pvId, OpenMode.ForRead) as ProfileView;
            
            // Obtenir les profils dans cette vue
            ObjectIdCollection profileIds = profileView.GetProfileIds();
            
            foreach (ObjectId profId in profileIds)
            {
                Profile profile = tr.GetObject(profId, OpenMode.ForRead) as Profile;
                // Travailler avec le profil
            }
        }
    }
    
    tr.Commit();
}
```

### 5. Accéder aux Styles et Paramètres

```csharp
// Accéder aux Styles Civil3D
CivilDocument civilDoc = CivilApplication.ActiveDocument;

// Styles d'alignement
ObjectIdCollection alignmentStyleIds = civilDoc.Styles.AlignmentStyles;

// Styles de surface
ObjectIdCollection surfaceStyleIds = civilDoc.Styles.SurfaceStyles;

// Styles de profil
ObjectIdCollection profileStyleIds = civilDoc.Styles.ProfileStyles;

// Styles de réseau de canalisations
ObjectIdCollection pipeStyleIds = civilDoc.Styles.PipeStyles;
ObjectIdCollection structureStyleIds = civilDoc.Styles.StructureStyles;

// Accéder aux Paramètres Civil3D
SettingsAlignment alignmentSettings = civilDoc.Settings.AlignmentSettings;
SettingsSurface surfaceSettings = civilDoc.Settings.SurfaceSettings;
```

---

## Modèles Communs

### Modèle de Transaction (CRITIQUE)

**Utilisez toujours des transactions** lors de l'accès aux objets de la base de données :

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    try
    {
        // Obtenir et travailler avec les objets
        
        tr.Commit(); // Valider les changements
    }
    catch (System.Exception ex)
    {
        // Gérer l'exception
        tr.Abort(); // Annuler en cas d'erreur
    }
}
```

### Modèle de Verrouillage de Document

Pour les commandes qui modifient le dessin, verrouillez le document :

```csharp
Document acDoc = Application.DocumentManager.MdiActiveDocument;
Database db = acDoc.Database;

using (DocumentLock docLock = acDoc.LockDocument())
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Modifier les objets ici
        
        tr.Commit();
    }
}
```

### Modèle d'Accès à l'Éditeur

Pour l'interaction utilisateur :

```csharp
Document acDoc = Application.DocumentManager.MdiActiveDocument;
Editor ed = acDoc.Editor;

// Demander une sélection
PromptSelectionResult selResult = ed.GetSelection();
if (selResult.Status == PromptStatus.OK)
{
    SelectionSet selSet = selResult.Value;
    // Travailler avec la sélection
}

// Écrire des messages
ed.WriteMessage("\nMessage sur la ligne de commande");
```

---

## Meilleures Pratiques

### 1. Toujours Utiliser des Transactions
N'accédez jamais aux objets de la base de données sans transaction.

### 2. Utiliser les Instructions `using`
Assurez-vous de disposer correctement des transactions et autres objets disposables.

### 3. Vérifier les Null
Vérifiez toujours si les objets existent avant de les utiliser :

```csharp
Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
if (ent != null)
{
    // Travailler avec l'entité
}
```

### 4. Utiliser le Mode d'Ouverture (OpenMode) Approprié
- `OpenMode.ForRead` - Pour lire les données seulement
- `OpenMode.ForWrite` - Pour modifier les objets

```csharp
// Lecture
Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;

// Modification
Entity ent = tr.GetObject(objId, OpenMode.ForWrite) as Entity;
ent.ColorIndex = 1; // Modifier la propriété
```

### 5. Mettre à Niveau le Mode d'Ouverture si Nécessaire

```csharp
Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
// Besoin de modifier plus tard
ent.UpgradeOpen();
ent.ColorIndex = 1;
```

### 6. Gérer les Collections d'ObjectId Efficacement

```csharp
// Bon - itérer directement
foreach (ObjectId objId in collection)
{
    Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
}

// Aussi bon - convertir en tableau si nécessaire plusieurs fois
ObjectId[] objIds = collection.Cast<ObjectId>().ToArray();
```

### 7. Utiliser la Vérification de Type avec Pattern Matching (C# 7.0+)

```csharp
if (ent is Line line)
{
    // Utiliser la variable 'line' directement
    double length = line.Length;
}
```

### 8. Gestion des Erreurs

```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    try
    {
        // Votre code ici
        tr.Commit();
    }
    catch (Autodesk.AutoCAD.Runtime.Exception ex)
    {
        ed.WriteMessage($"\nErreur AutoCAD : {ex.Message}");
        tr.Abort();
    }
    catch (System.Exception ex)
    {
        ed.WriteMessage($"\nErreur Générale : {ex.Message}");
        tr.Abort();
    }
}
```

### 9. Travailler avec AutoCAD et Civil3D

```csharp
// Vérifier si Civil3D est disponible
bool isCivil3D = CivilApplication.ActiveDocument != null;

if (isCivil3D)
{
    CivilDocument civilDoc = CivilApplication.ActiveDocument;
    // Travailler avec les objets Civil3D
}
else
{
    // Travailler avec les objets AutoCAD seulement
}
```

---

## Exemple Complet : Scanner Tous les Objets

```csharp
[CommandMethod("SCANALL")]
public void ScanAllObjects()
{
    Document acDoc = Application.DocumentManager.MdiActiveDocument;
    Database db = acDoc.Database;
    Editor ed = acDoc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        try
        {
            // Scanner les entités AutoCAD
            BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
            BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
                OpenMode.ForRead) as BlockTableRecord;
            
            ed.WriteMessage("\n=== Entités AutoCAD ===");
            foreach (ObjectId objId in modelSpace)
            {
                Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
                ed.WriteMessage($"\n{ent.GetType().Name}");
            }
            
            // Scanner les objets Civil3D (si disponible)
            CivilDocument civilDoc = CivilDocument.GetCivilDocument(db);
            if (civilDoc != null)
            {
                ed.WriteMessage("\n\n=== Objets Civil3D ===");
                
                // Alignements
                foreach (ObjectId alignId in civilDoc.GetAlignmentIds())
                {
                    Alignment align = tr.GetObject(alignId, OpenMode.ForRead) as Alignment;
                    ed.WriteMessage($"\nAlignement : {align.Name}");
                }
                
                // Surfaces
                foreach (ObjectId surfId in civilDoc.GetSurfaceIds())
                {
                    Surface surf = tr.GetObject(surfId, OpenMode.ForRead) as Surface;
                    ed.WriteMessage($"\nSurface : {surf.Name} ({surf.GetType().Name})");
                }
                
                // Ajouter plus de types d'objets Civil3D au besoin...
            }
            
            tr.Commit();
        }
        catch (System.Exception ex)
        {
            ed.WriteMessage($"\nErreur : {ex.Message}");
            tr.Abort();
        }
    }
}
```

---

## Références

- [Guide du Développeur AutoCAD .NET](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Référence API Civil3D .NET](https://help.autodesk.com/view/CIV3D/2024/ENU/)
- [Documentation SDK ObjectARX](https://www.autodesk.com/developer-network/platform-technologies/autocad)
