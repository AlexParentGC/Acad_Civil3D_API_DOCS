# Classe SymbolTableRecord

## Vue d'Ensemble
`SymbolTableRecord` est la classe de base abstraite pour tous les objets stockés dans une `SymbolTable`. Des exemples courants incluent `LayerTableRecord` (Calques), `BlockTableRecord` (Définitions de Bloc), `LinetypeTableRecord`, et `TextStyleTableRecord`. Elle définit les propriétés communes pour les éléments de table nommés, telles que le nom de l'entrée.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTableRecord
              ├─ LayerTableRecord
              ├─ BlockTableRecord
              ├─ LinetypeTableRecord
              ├─ TextStyleTableRecord
              ├─ DimStyleTableRecord
              ├─ RegAppTableRecord
              ├─ UcsTableRecord
              ├─ ViewTableRecord
              └─ ViewportTableRecord
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Le nom de l'enregistrement (ex : Nom de calque "0"). |
| `IsDependent` | `bool` | Vrai si l'enregistrement appartient à une XRef. |
| `IsResolved` | `bool` | Vrai si l'enregistrement XRef est résolu. |
| `IsRenamable` | `bool` | Vrai si l'enregistrement peut être renommé. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetName()` | `string` | Obtient le nom en toute sécurité. |

## Exemples de Code

### Exemple 1: Créer un Nouvel Enregistrement (Générique)
```csharp
// Typiquement vous instanciez une classe dérivée, pas SymbolTableRecord directement
LayerTableRecord layer = new LayerTableRecord();
layer.Name = "MyNewLayer";
// Définir d'autres propriétés spécifiques...
```

### Exemple 2: Renommer un Enregistrement
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTableRecord rec = tr.GetObject(recordId, OpenMode.ForWrite) as SymbolTableRecord;
    
    if (rec.IsRenamable) // Vérifier la protection système (ex : Calque "0")
    {
        rec.Name = "RenamedRecord";
    }
    
    tr.Commit();
}
```

### Exemple 3: Vérifier le Statut XRef
```csharp
public void CheckDependency(SymbolTableRecord rec)
{
    if (rec.IsDependent)
    {
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\n{rec.Name} est un enregistrement dépendant d'une XRef.");
    }
}
```

### Exemple 4: Itérer un BlockTableRecord (Hérité)
```csharp
// BlockTableRecord est un SymbolTableRecord, mais aussi un conteneur !
// Note : Seul BlockTableRecord se comporte comme un conteneur d'Entités.
// Les autres enregistrements (Calque, Type de Ligne) sont juste des définitions.
```

### Exemple 5: Gérer la Validation de Nom
```csharp
public void SetName(SymbolTableRecord rec, string newName)
{
    try
    {
        // SymbolTableRecord valide les noms (pas de caractères invalides)
        rec.Name = newName;
    }
    catch (Exception ex)
    {
        // Gérer l'erreur de nom invalide
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nCaractères de nom invalides.");
    }
}
```

### Exemple 6: Cloner un Enregistrement
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTableRecord original = tr.GetObject(id, OpenMode.ForRead) as SymbolTableRecord;
    SymbolTableRecord clone = original.Clone() as SymbolTableRecord;
    
    clone.Name = original.Name + "_Copie";
    
    // Vous ajouteriez ensuite 'clone' à la table propriétaire...
    tr.Commit();
}
```

### Exemple 7: Utiliser comme DBObject
```csharp
// Puisqu'il hérite de DBObject, toutes les méthodes DBObject s'appliquent
SymbolTableRecord rec = tr.GetObject(id, OpenMode.ForRead) as SymbolTableRecord;
ObjectId ownerId = rec.OwnerId; // La SymbolTable (ex : LayerTable)
```

### Exemple 8: Identifier le Type d'Enregistrement
```csharp
if (rec is LayerTableRecord) return "Calque";
if (rec is BlockTableRecord) return "Bloc";
if (rec is LinetypeTableRecord) return "Type de Ligne";
```

## Meilleures Pratiques
1. **Validation de Nom** : Soyez conscient que définir `Name` lance des exceptions si le nom contient des caractères invalides (`<`, `>`, `/`, `:`, etc.).
2. **Sécurité XRef** : les enregistrements dépendants (`IsDependent == true`) sont généralement en lecture seule ; tenter de les modifier ou de les renommer échouera.
3. **Vérification Renommable** : Vérifiez toujours `IsRenamable` avant de changer `Name` (certains enregistrements système comme "0" ou "Defpoints" sont verrouillés).

## Objets Associés
- [SymbolTable](SymbolTable.md) - Le conteneur pour ces enregistrements.
- [LayerTable](../SymbolTables/LayerTable.md) - Exemple de table.
- [LayerTableRecord](../SymbolTables/LayerTableRecord.md) - Exemple d'enregistrement dérivé.

## Références
- [Référence SymbolTableRecord Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_SymbolTableRecord)
