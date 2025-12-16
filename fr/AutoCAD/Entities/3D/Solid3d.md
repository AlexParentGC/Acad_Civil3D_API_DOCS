# Classe Solid3d

## Vue d'Ensemble
`Solid3d` représente un volume primitif 3D ou un solide ACIS dans la base de données AutoCAD. C'est la classe principale pour créer une géométrie 3D complexe en utilisant des opérations booléennes (Union, Soustraction, Intersection) et des primitives (Boîte, Sphère, Cylindre, etc.).

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Solid3d
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Mass` | `double` | Volume du solide. |
| `Area` | `double` | Aire de surface. |
| `Centroid` | `Point3d` | Centre de masse. |
| `MomentsOfInertia` | `Vector3d` | Moments d'inertie. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `CreateBox(double, double, double)` | `void` | Crée un solide boîte. |
| `CreateSphere(double)` | `void` | Crée un solide sphère. |
| `CreateCylinder(double, double)` | `void` | Crée un cylindre. |
| `CreateExtrudedSolid(Entity, Vector3d, SweepOptions)` | `void` | Extrude un profil. |
| `BooleanOperation(BooleanOperationType, Solid3d)` | `void` | Effectue Union/Soustraction/Intersection. |
| `Slice(Plane, bool)` | `Solid3d` | Coupe le solide par un plan. |

## Exemples de Code

### Exemple 1: Créer une Boîte Simple
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Solid3d box = new Solid3d();
    box.CreateBox(10.0, 10.0, 10.0); // Cube 10x10x10
    
    // Positionner à 5,5,5
    box.TransformBy(Matrix3d.Displacement(new Vector3d(5, 5, 5)));
    
    btr.AppendEntity(box);
    tr.AddNewlyCreatedDBObject(box, true);
    
    tr.Commit();
}
```

### Exemple 2: Soustraction Booléenne (Boîte Creuse)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Boîte extérieure
    Solid3d outer = new Solid3d();
    outer.CreateBox(12, 12, 12);
    
    // Boîte intérieure
    Solid3d inner = new Solid3d();
    inner.CreateBox(10, 10, 10);
    
    // Utiliser la soustraction booléenne : Extérieur - Intérieur
    outer.BooleanOperation(BooleanOperationType.BoolSubtract, inner);
    
    // Le résultat est une boîte creuse (si centrée)
    // Note : 'inner' est consommé/invalidé par l'opération à moins que vous ne l'ayez cloner,
    // mais ici nous le libérons juste implicitement ou explicitement.
    // En fait, BooleanOperation détruit généralement le second opérande.
    // Vous n'ajoutez que 'outer' à la base de données.
    inner.Dispose();
    
    btr.AppendEntity(outer);
    tr.AddNewlyCreatedDBObject(outer, true);
    
    tr.Commit();
}
```

### Exemple 3: Extruder un Cercle (Cylindre)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer Profil
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    
    // Créer Solide
    Solid3d cylinder = new Solid3d();
    cylinder.CreateExtrudedSolid(circle, Vector3d.ZAxis * 10, new SweepOptions());
    
    btr.AppendEntity(cylinder);
    tr.AddNewlyCreatedDBObject(cylinder, true);
    
    tr.Commit();
}
```

### Exemple 4: Obtenir les Propriétés de Masse
```csharp
public void LogMassProps(Solid3d solid)
{
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage(
        $"\nVolume : {solid.Mass:F2}" +
        $"\nAire : {solid.Area:F2}" +
        $"\nCentroïde : {solid.Centroid}"
    );
}
```

### Exemple 5: Couper un Solide
```csharp
Solid3d solid = ...;
Plane slicePlane = new Plane(Point3d.Origin, Vector3d.XAxis); // Coupe à X=0

// Effectuer la coupe
// solid est modifié pour être le côté "négatif" du plan
// Retourne le côté "positif" comme un nouveau Solid3d
Solid3d otherHalf = solid.Slice(slicePlane, negativeHalfToo: true);

if (otherHalf != null)
{
    // Ajouter l'autre moitié à la base de données
}
```

### Exemple 6: Vérification d'Interférence
```csharp
Solid3d sol1 = ...;
Solid3d sol2 = ...;

bool interference = sol1.CheckInterference(sol2);

if (interference)
{
    // Créer le solide d'interférence
    Solid3d intersection = sol1.Clone() as Solid3d;
    intersection.BooleanOperation(BooleanOperationType.BoolIntersect, sol2);
    // 'intersection' est maintenant le volume commun
}
```

### Exemple 7: Création à partir de Région (Révolution)
```csharp
Region reg = ...; // Profil 2D
Solid3d revSolid = new Solid3d();

// Révolution de la région de 360 degrés autour de l'axe Y
revSolid.CreateRevolvedSolid(reg, Point3d.Origin, Vector3d.YAxis, Math.PI * 2, 0.0, new RevolveOptions());
```

### Exemple 8: Chanfreiner des Arêtes (Brep)
```csharp
// Avancé : La manipulation des arêtes ACIS nécessite souvent l'API Brep ou des méthodes spécifiques de Solid3d
// Les modifications simples sont généralement effectuées via des opérations booléennes.
// Le chanfreinage direct des arêtes via l'API est complexe et implique souvent l'accès au BRep interne.
```

## Meilleures Pratiques
1. **Libération (Disposal)** : Les objets `Solid3d` consomment une mémoire non gérée significative (ACIS). Toujours faire `Dispose()` s'ils sont créés en mémoire et non ajoutés à la base de données.
2. **Opérations Booléennes** : Le second opérande dans `BooleanOperation` est essentiellement détruit/consommé. N'essayez pas de le réutiliser après l'opération.
3. **Propriétés de Masse** : Calculer les propriétés de masse (`Mass`, `Centroid`) peut être coûteux en calcul pour des solides complexes. Mettez en cache les valeurs si nécessaire.
4. **Historique** : L'historique des solides (enregistrement des étapes) peut gonfler la taille du fichier. Vérifiez la propriété `RecordHistory`.

## Objets Associés
- [Region](Region.md) - Équivalent 2D.
- [Body](Body.md) - Wrapper de corps Filaire/Feuille.
- [Region](../3D/Region.md)

## Références
- [Référence Autodesk Solid3d](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Solid3d)
