# Classe Body

## Vue d'Ensemble
`Body` est un conteneur générique pour un corps ACIS qui ne rentre pas strictement dans les catégories `Solid3d` (volume solide) ou `Region` (zone plane). Il est souvent utilisé pour :
- Corps de feuille (surfaces).
- Corps filaires (filaire 3D).
- Corps résultant d'opérations booléennes échouées ou d'états indéterminés.
- Conversion entre d'autres formats 3D.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Body
```

## Propriétés Clés
*Hérite de la plupart des propriétés de Entity.*

| Propriété | Type | Description |
|-----------|------|-------------|
| `IsNull` | `bool` | Vrai si le pointeur ACIS interne est nul. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SetBody(IntPtr)` | `void` | Définit le pointeur de corps ACIS interne (Avancé). |
| `GetBody()` | `IntPtr` | Obtient le pointeur de corps ACIS interne. |

## Exemples de Code

### Exemple 1: Vérification de création d'un Body
```csharp
// Les corps sont rarement créés directement via "new Body()".
// Ils résultent généralement d'opérations de modélisation ou d'importation.
Body body = ent as Body;
if (body != null)
{
    // C'est un corps ACIS générique
}
```

### Exemple 2: Conversion de Solid en Body
```csharp
// Parfois, une analyse détaillée nécessite de traiter un solide comme un corps générique
// C'est plus courant en ObjectARX (C++) qu'en .NET
```

### Exemple 3: Utiliser Body pour BREP (Représentation par Limites)
```csharp
using Autodesk.AutoCAD.BoundaryRepresentation;

// Utiliser l'API BREP pour analyser la topologie
using (Brep brep = new Brep(someEntity))
{
    foreach (Autodesk.AutoCAD.BoundaryRepresentation.Face face in brep.Faces)
    {
        // Analyser les faces du body/solide/région
    }
}
```

### Exemple 4: Identifier le Type ACIS
```csharp
// Comme Body est générique, il est difficile de savoir ce que c'est (Feuille ? Fil ?)
// Souvent la vérification des propriétés de masse ou l'utilisation de l'API Brep est nécessaire.
```

### Exemple 5: Exploser des imports complexes
```csharp
// Les fichiers STEP/IGES importés arrivent souvent comme Bodies
// Les exploser peut donner des Surfaces ou des Courbes
DBObjectCollection parts = new DBObjectCollection();
body.Explode(parts);
```

### Exemple 6: Vérification de Validité
```csharp
if (!body.IsNull) 
{ 
    // valide 
}
```

### Exemple 7: Intégration Modélisation ACIS
```csharp
// Body est souvent utilisé comme pont vers des noyaux externes ou des fichiers SAT hérités
```

### Exemple 8: Gestion des Corps "Feuille"
```csharp
// Un "Corps Feuille" (surface) en termes ACIS est souvent enveloppé comme une entité Body dans AutoCAD
// plutôt qu'une entité Surface (qui est plus récente).
```

## Meilleures Pratiques
1. **Préférer les Types Spécifiques** : Utilisez les classes `Solid3d` ou `Surface` si possible. `Body` est un repli fourre-tout.
2. **API Brep** : Pour faire quelque chose d'utile avec un `Body` (mesurer, analyser), vous avez généralement besoin du namespace `Autodesk.AutoCAD.BoundaryRepresentation`.
3. **Performance** : Comme tous les wrappers ACIS, ils sont lourds. Libérez (Dispose) quand c'est fini.

## Objets Associés
- [Solid3d](Solid3d.md)
- [Region](Region.md)

## Références
- [Référence Autodesk Body](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Body)
