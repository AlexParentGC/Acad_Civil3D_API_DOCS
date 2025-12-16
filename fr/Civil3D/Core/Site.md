# Classe Site

## Vue d'Ensemble
`Site` est un conteneur topologique fondamental dans Civil 3D. Il agit comme un seau pour les objets liés qui interagissent entre eux, spécifiquement les **Parcelles**, **Axes**, et **Terrassements**. Les objets au sein du même Site interagissent automatiquement (par exemple, les axes subdivisent les parcelles).

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Site
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Nom du site. |
| `Parcels` | `ObjectId` | ID de la collection de parcelles. |
| `Alignments` | `ObjectId` | ID de la collection d'axes. |
| `FeatureLines` | `ObjectId` | ID de la collection de lignes caractéristiques. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Create(CivilDocument, string)` | `ObjectId` | Méthode statique pour créer un nouveau site. |

## Exemples de Code

### Exemple 1: Créer un Nouveau Site
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    CivilDocument civDoc = CivilApplication.ActiveDocument;
    
    ObjectId siteId = Site.Create(civDoc, "Phase 1 Développement");
    
    // Le site est maintenant prêt à accepter des parcelles/axes
    tr.Commit();
}
```

### Exemple 2: Itérer les Sites Existants
```csharp
CivilDocument civDoc = CivilApplication.ActiveDocument;

foreach (ObjectId siteId in civDoc.GetSiteIds())
{
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Site site = tr.GetObject(siteId, OpenMode.ForRead) as Site;
        ed.WriteMessage($"\nSite : {site.Name}");
    }
}
```

### Exemple 3: Trouver un Site principalement par Nom
```csharp
public ObjectId FindSite(string name)
{
    CivilDocument civDoc = CivilApplication.ActiveDocument;
    foreach (ObjectId id in civDoc.GetSiteIds())
    {
        Site s = id.GetObject(OpenMode.ForRead) as Site;
        if (s.Name.Equals(name, StringComparison.OrdinalIgnoreCase))
            return id;
    }
    return ObjectId.Null;
}
```

### Exemple 4: Obtenir Collection de Parcelles
```csharp
// L'objet Site n'expose PAS directement une collection "Parcels"
// Au lieu de cela, vous accédez généralement aux parcelles via le CivilDocument ou en vérifiant les collections de dépendance du Site.
// Approche correcte : Accéder aux collections via l'ID du site.
```

### Exemple 5: Déplacer des Objets vers un Site
```csharp
// Vous ne "déplacez" pas les objets ; vous les créez dans le site.
// Changer le site d'une Parcelle implique généralement une recréation ou des APIs MoveToSite spécifiques si disponibles.
```

### Exemple 6: Supprimer un Site
```csharp
// Usage DBObject standard
site.Erase(); // Supprime le site ET son contenu topologique
```

### Exemple 7: Renommer
```csharp
site.Name = "Nouveau Nom Site";
```

### Exemple 8: Logique de Topologie
```csharp
// Objets dans ce site :
// 1. Les Axes coupent les Parcelles
// 2. Les Groupes de Terrassement interagissent
// 3. Les objets dans des sites DIFFÉRENTS n'interagissent pas.
```

## Meilleures Pratiques
1. **Séparation** : Utilisez différents Sites pour empêcher les interactions indésirables. Par exemple, gardez les axes "Conditions Existantes" séparés des parcelles "Projetées" si vous ne voulez pas qu'ils se subdivisent.
2. **Site "Aucun"** : Certains objets peuvent être "sans site" (Site `ObjectId.Null`). Cela empêche toute interaction topologique.

## Objets Associés
- [Parcel](../Parcels/Parcel.md) - Parcelle
- [Alignment](../Alignment/Alignment.md) - Axe

## Références
- [Référence Site Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/)
