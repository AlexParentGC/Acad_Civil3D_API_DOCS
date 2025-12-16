# Classe DBObjectCollection

## Vue d'Ensemble
`DBObjectCollection` est un conteneur qui détient des références à des instances réelles de `DBObject` (pas seulement leurs IDs). Il est typiquement utilisé pour des opérations impliquant des objets non résidents de la base de données (objets créés en mémoire mais pas encore ajoutés à la base de données), tels que `Entity.Explode()` ou `Curve.GetSplitCurves()`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Count` | `int` | Nombre d'objets dans la collection. |
| `this[int]` | `DBObject` | Indexeur pour obtenir l'objet à l'index. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Add(DBObject)` | `int` | Ajoute un objet à la collection. |
| `Clear()` | `void` | Vide la collection (ne supprime pas les objets). |
| `Contains(DBObject)` | `bool` | Vérifie si l'objet est dans la liste. |
| `CopyTo(DBObject[], int)` | `void` | Copie vers un tableau. |
| `Dispose()` | `void` | Libère le conteneur de collection. |

## Exemples de Code

### Exemple 1: Décomposer une Entité
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entId, OpenMode.ForRead) as Entity;
    
    // Créer une collection spécifique pour les résultats
    DBObjectCollection explodedParts = new DBObjectCollection();
    
    // Décomposer dans la collection
    ent.Explode(explodedParts);
    
    ed.WriteMessage($"\nDécomposé en {explodedParts.Count} pièces.");
    
    // Ajouter les nouvelles pièces à la base de données
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    foreach (Entity part in explodedParts)
    {
        btr.AppendEntity(part);
        tr.AddNewlyCreatedDBObject(part, true);
    }
    
    tr.Commit();
}
```

### Exemple 2: Diviser des Courbes
```csharp
Curve curve = ...;
Point3dCollection splitPoints = ...;

DBObjectCollection newCurves = new DBObjectCollection();

// Diviser la courbe aux points
// Les courbes résultantes sont ajoutées à newCurves
curve.GetSplitCurves(splitPoints, newCurves);
```

### Exemple 3: Créer des Régions
```csharp
// Region.CreateFromCurves nécessite une entrée DBObjectCollection
DBObjectCollection curves = new DBObjectCollection();
curves.Add(new Line(new Point3d(0,0,0), new Point3d(10,0,0)));
curves.Add(new Line(new Point3d(10,0,0), new Point3d(10,10,0)));
// (ajouter plus de courbes fermantes)

DBObjectCollection regions = Region.CreateFromCurves(curves);
```

### Exemple 4: Libération Correcte
```csharp
// IMPORTANT : DBObjectCollection est IDisposable
using (DBObjectCollection list = new DBObjectCollection())
{
    // remplir liste
    // utiliser liste
} // La collection est libérée ici
```

### Exemple 5: Gérer les Objets en Mémoire
```csharp
// Les objets dans DBObjectCollection ne sont souvent PAS encore dans la base de données.
// Si vous ne les ajoutez pas à la base de données, vous devez les libérer manuellement !

DBObjectCollection parts = new DBObjectCollection();
someEntity.Explode(parts);

// ... décider de ne pas les utiliser ...

foreach (DBObject obj in parts)
{
    obj.Dispose(); // Nettoyage mémoire
}
parts.Dispose(); // Nettoyage conteneur
```

## Meilleures Pratiques
1. **Gestion de la Mémoire** : C'est le piège n°1. `DBObjectCollection` ne possède **PAS** les objets à l'intérieur. Si ces objets ne sont pas physiquement ajoutés à la Database (via `AppendEntity`), vous êtes responsable d'appeler `.Dispose()` sur chaque objet à l'intérieur, OU la mémoire fuit.
2. **Libérer la Collection** : La collection elle-même utilise des ressources non gérées, donc utilisez toujours `using` ou `Dispose()`.
3. **Utiliser les Types API** : N'utilisez pas `List<DBObject>` quand une méthode API attend `DBObjectCollection` - vous devez utiliser le type AutoCAD.

## Objets Associés
- [DBObject](../BaseClasses/DBObject.md) - Le type d'élément.
- [ObjectIdCollection](ObjectIdCollection.md) - Collection de références (poignées).

## Références
- [Référence DBObjectCollection Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBObjectCollection)
