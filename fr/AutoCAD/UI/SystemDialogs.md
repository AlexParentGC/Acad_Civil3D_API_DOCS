# Classe SystemDialogs

## Vue d'Ensemble
Cette documentation couvre les boîtes de dialogue système standard fournies par le namespace `Autodesk.AutoCAD.Windows`, permettant aux développeurs d'invoquer les sélecteurs natifs d'AutoCAD pour les fichiers, couleurs et types de ligne.

## Namespace
`Autodesk.AutoCAD.Windows`

## Classes Clés

| Classe | Description |
|--------|-------------|
| `OpenFileDialog` | Dialogue d'ouverture de fichier standard. |
| `SaveFileDialog` | Dialogue d'enregistrement de fichier standard. |
| `ColorDialog` | Sélecteur de couleur AutoCAD (ACI) et TrueColor. |
| `LinetypeDialog` | Dialogue de sélection de type de ligne. |
| `LayerDialog` | Dialogue de sélection de calque (via UI interne ou personnalisé). |

## Exemples de Code

### Exemple 1: Dialogue Ouvrir Fichier
```csharp
using Autodesk.AutoCAD.Windows;

OpenFileDialog ofd = new OpenFileDialog(
    "Sélectionner Configuration", 
    null, 
    "xml;json", 
    "FichiersParametres", 
    OpenFileDialog.OpenFileDialogFlags.DoNotTransferRemoteFiles
);

System.Windows.Forms.DialogResult dr = ofd.ShowDialog();

if (dr == System.Windows.Forms.DialogResult.OK)
{
    string selectedFile = ofd.Filename;
    // Traiter fichier
}
```

### Exemple 2: Dialogue Enregistrer Fichier
```csharp
SaveFileDialog sfd = new SaveFileDialog(
    "Exporter Données",
    "rapport",
    "csv",
    "FichierExport",
    SaveFileDialog.SaveFileDialogFlags.None
);

if (sfd.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    string savePath = sfd.Filename;
}
```

### Exemple 3: Sélecteur de Couleur
```csharp
ColorDialog cd = new ColorDialog();
cd.Color = Autodesk.AutoCAD.Colors.Color.FromColorIndex(ColorMethod.ByAci, 1); // Rouge par défaut
cd.IncludeByBlockByLayer = true; // Permettre DuBloc/DuCalque

if (cd.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    Autodesk.AutoCAD.Colors.Color selected = cd.Color;
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nSélectionné : {selected.ColorIndex}");
}
```

### Exemple 4: Sélecteur de Type de Ligne
```csharp
LinetypeDialog ltd = new LinetypeDialog();
// ltd.LinetypeId = ...; // Définir défaut

if (ltd.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    ObjectId linetypeId = ltd.LinetypeId;
    // Appliquer type de ligne
}
```

### Exemple 5: MessageBox (Standard)
```csharp
// AutoCAD fournit une intégration standard MessageBox mais généralement vous utilisez Forms/WPF standard
// Pour une alerte style AutoCAD :
Application.ShowAlertDialog("Erreur Critique Survenue !");
```

### Exemple 6: Dialogue État de Calque (Avancé)
```csharp
// Il n'y a pas de classe "LayerDialog" simpliste exposée directement comme ColorDialog.
// Généralement les développeurs construisent un formulaire personnalisé listant les calques de la base de données,
// ou utilisent l'interop COM pour invoquer les dialogues intégrés.
```

### Exemple 7: Gérer les Résultats de Dialogue
```csharp
// Les dialogues retournent System.Windows.Forms.DialogResult
// Rappelez-vous de référencer System.Windows.Forms
if (result == System.Windows.Forms.DialogResult.Cancel)
{
    // Utilisateur a annulé
    return;
}
```

### Exemple 8: Navigateur de Dossier
```csharp
// Pour la sélection de dossier, utilisez le standard .NET 
// System.Windows.Forms.FolderBrowserDialog
// AutoCAD ne fournit pas de surcharge spécifique pour cela.
```

## Meilleures Pratiques
1. **Contexte Modal** : Ces dialogues sont modaux. Ils bloquent l'interface utilisateur AutoCAD jusqu'à la fermeture.
2. **Références** : Nécessite la référence `System.Windows.Forms` dans votre projet.
3. **Thread UI** : Affichez toujours les dialogues depuis le thread UI principal.

## Objets Associés
- [Editor](../Core/Editor.md)
- [Application](../Core/Application.md)

## Références
- [Namespace Windows Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows)
