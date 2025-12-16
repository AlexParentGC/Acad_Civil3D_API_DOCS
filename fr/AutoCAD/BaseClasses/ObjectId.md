# Structure ObjectId

## Vue d'Ensemble
La structure `ObjectId` est l'identifiant robuste et persistant pour tout `DBObject` résidant dans une base de données AutoCAD. Contrairement à un pointeur mémoire, un `ObjectId` reste valide à travers les transactions et la réallocation de mémoire. C'est le handle principal utilisé pour récupérer des objets depuis le `TransactionManager`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `IsNull` | `bool` | Vrai si l'ID n'est pas valide/nul. |
| `IsValid` | `bool` | Vrai si l'ID pointe vers un objet valide. |
| `IsErased` | `bool` | Vrai si l'objet cible est effacé. |
| `ObjectClass` | `RXClass` | Le type de classe d'exécution de l'objet cible. |
| `Database` | `Database` | La base de données possédant cet objet. |
| `Handle` | `Handle` | Le handle persistant de l'objet cible. |
| `OriginalDatabase` | `Database` | La base de données d'où l'objet provient (pour les XRefs). |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetObject(OpenMode)` | `DBObject` | **Obsolète**. Utilisez `Transaction.GetObject` à la place. |
| `Equals(object)` | `bool` | Vérifie l'égalité avec un autre ObjectId. |
| `GetHashCode()` | `int` | Retourne le code de hachage pour les clés de dictionnaire. |
| `ToString()` | `string` | Retourne la représentation sous forme de chaîne. |

## Exemples de Code

### Exemple 1: Vérifier un ObjectId Nul
```csharp
public void CheckId(ObjectId id)
{
    if (id.IsNull)
    {
        // Gérer le cas d'ID nul
        return;
    }
    
    // Continuer avec ID valide
}
```

### Exemple 2: Obtenir le Type d'Objet depuis l'ObjectId
```csharp
public void CheckType(ObjectId id)
{
    // Vérifier le type sans ouvrir l'objet (RAPIDE)
    if (id.ObjectClass.IsDerivedFrom(RXObject.GetClass(typeof(Curve))))
    {
        // C'est une courbe (Ligne, Arc, Polyligne, etc.)
    }
}
```

### Exemple 3: Utiliser ObjectId dans une Transaction
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Meilleure Pratique : Toujours utiliser Transaction.GetObject
    Entity ent = tr.GetObject(objectId, OpenMode.ForRead) as Entity;
    if (ent != null)
    {
        // Travailler avec l'entité
    }
    tr.Commit();
}
```

### Exemple 4: Comparer des ObjectIds
```csharp
public bool IsSameObject(ObjectId id1, ObjectId id2)
{
    // Opérateur de comparaison directe
    return id1 == id2;
}
```

### Exemple 5: Vérification de Validité d'ObjectId
```csharp
public void ValidateId(ObjectId id)
{
    if (!id.IsValid)
    {
        // L'ID peut provenir d'une base de données supprimée ou être invalide
        throw new ArgumentException("ObjectId Invalide");
    }

    if (id.IsErased)
    {
        // L'objet existe mais est supprimé (soft-deleted)
        // Vous pouvez toujours l'ouvrir avec OpenMode.ForRead si nécessaire, mais généralement on ignore.
    }
}
```

### Exemple 6: Extraire le Handle
```csharp
public void LogHandle(ObjectId id)
{
    // Le Handle est l'identifiant chaîne persistant (ex : "5A")
    Handle h = id.Handle;
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nHandle : {h}");
}
```

### Exemple 7: Utiliser comme Clé de Dictionnaire
```csharp
// ObjectId implémente GetHashCode et Equals correctement
Dictionary<ObjectId, string> objectTags = new Dictionary<ObjectId, string>();

objectTags[id1] = "Inspecté";
if (objectTags.ContainsKey(id1))
{
    // Trouvé
}
```

### Exemple 8: Vérifier la Base de Données d'Origine (XRef)
```csharp
public void CheckXRef(ObjectId id)
{
    if (id.Database != id.OriginalDatabase)
    {
        // L'objet provient probablement d'une XRef
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nObjet XRef Détecté");
    }
}
```

## Meilleures Pratiques
1. **Ne jamais utiliser `Handle` pour la logique d'exécution** : Utilisez `ObjectId` quand c'est possible car c'est plus rapide. Utilisez `Handle` uniquement pour la persistance entre les sessions.
2. **Éviter `Open()`** : N'utilisez pas `ObjectId.Open()`. Utilisez toujours `Transaction.GetObject()`.
3. **Vérifier `IsNull`** : Vérifiez toujours `ObjectId.Null` avant de passer des IDs.
4. **Utiliser `ObjectClass`** : Vérifiez le type d'objet via `id.ObjectClass` pour éviter d'ouvrir des objets juste pour vérifier leur type.

## Objets Associés
- [DBObject](DBObject.md) - L'objet pointé par l'ObjectId.
- [Database](../Core/Database.md) - Le conteneur de l'objet.
- [Handle](https://help.autodesk.com/view/OARX/2024/ENU/) - Identifiant persistant.

## Références
- [Référence ObjectId Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ObjectId)
