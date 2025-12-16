# Classe Parcel

## Vue d'Ensemble
`Parcel` représente une zone fermée 2D dans Civil 3D, représentant typiquement une parcelle de terrain ou une emprise. Les parcelles sont des entités topologiques dynamiques ; elles sont automatiquement formées et mises à jour par la géométrie (Segments de Parcelle ou Axes) au sein de leur `Site` parent.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Parcel
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Area` | `double` | Aire de la parcelle. |
| `Perimeter` | `double` | Périmètre total. |
| `Name` | `string` | Nom/numéro de la parcelle. |
| `Number` | `int` | Numéro de parcelle automatique. |
| `SiteId` | `ObjectId` | Le site auquel elle appartient. |
| `UserDefinedProperties` | `UDP` | Conteneur de propriétés personnalisées. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `Create(...)` | `ObjectId` | Méthodes statiques pour créer à partir d'objets. |
| `CreateFromObjects(...)` | `ObjectId` | Crée des parcelles à partir de polylignes CAO. |

## Exemples de Code

### Exemple 1: Créer des Parcelles à partir de Polylignes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectId siteId = ...; // Obtenir Site cible
    ObjectIdCollection polyIds = ...; // Polylignes à convertir
    
    ObjectIdCollection createdParcels = Parcel.CreateFromObjects(
        polyIds, 
        siteId, 
        db.CurrentSpaceId, 
        false // Effacer les entités existantes ?
    );
    
    ed.WriteMessage($"\nCréé {createdParcels.Count} parcelles.");
    tr.Commit();
}
```

### Exemple 2: Lire l'Aire de la Parcelle
```csharp
Parcel p = tr.GetObject(id, OpenMode.ForRead) as Parcel;
ed.WriteMessage($"\nParcelle {p.Name} : Aire = {p.Area:F2} pi carrés");
```

### Exemple 3: Renuméroter les Parcelles
```csharp
// La renumérotation est généralement une opération au niveau du Site ou via les propriétés
p.Name = "Lot " + p.Number;
```

### Exemple 4: Obtenir la Géométrie des Segments
```csharp
// Les parcelles ne possèdent pas la géométrie directement ; elles sont définies PAR des segments.
// Pour obtenir la forme, vous pouvez regarder les courbes de base.
```

### Exemple 5: Travailler avec UDP (Propriétés Définies par l'Utilisateur)
```csharp
// Civil 3D permet des champs personnalisés sur les Parcelles (ex : "Zonage", "Propriétaire")
// Accéder à ceux-ci nécessite l'API UDP (UserDefinedPropertyClassification)
// C'est avancé et implique `Parcel.GetUserDefinedPropertyValue`.
```

### Exemple 6: Mises à Jour Topologiques
```csharp
// Si vous dessinez un Axe à travers cette Parcelle (dans le même Site),
// La Parcelle se divisera en deux AUTOMATIQUEMENT.
// L'API reflète cela : l'objet Parcelle original rétrécit généralement,
// et un nouvel objet Parcelle est créé pour la partie divisée.
```

### Exemple 7: Vérification Inverse/Map
```csharp
// Obtenir la description légale de la géométrie
// Itérer les segments...
```

### Exemple 8: Styles
```csharp
p.StyleId = newStyleId; // Changer le style visuel (ex : "Unifamilial")
p.AreaLabelStyleId = newLabelStyleId; // Changer le style d'étiquette
```

## Meilleures Pratiques
1. **Conscience Topologique** : Rappelez-vous que modifier UN segment peut affecter DEUX parcelles (bord partagé).
2. **Gestion de Site** : Assurez-vous de travailler dans le bon Site. Les parcelles ne peuvent pas exister sans un Site.
3. **Mises à Jour Automatiques** : Parce que les Parcelles réagissent à d'autres géométries, les GUIDs/IDs sont stables, mais la géométrie est volatile.

## Objets Associés
- [Site](../Core/Site.md) - Le conteneur.
- [Alignment](../Alignment/Alignment.md) - Peut former des limites de parcelle.

## Références
- [Référence Parcelle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/)
