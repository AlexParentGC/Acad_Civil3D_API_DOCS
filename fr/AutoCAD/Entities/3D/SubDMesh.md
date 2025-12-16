# Classe SubDMesh

## Vue d'Ensemble
`SubDMesh` (Maillage de Subdivision) représente un objet maillage moderne capable de lissage, de plissage et de modélisation organique complexe. Contrairement au `PolygonMesh` hérité, il supporte les faces n-gon, des niveaux de lissage variés et la pondération des arêtes/sommets.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ SubDMesh
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `SubdivisionLevel` | `int` | Niveau de lissage actuel (0=non lissé). |
| `VertexCount` | `int` | Nombre de sommets. |
| `FaceCount` | `int` | Nombre de faces. |
| `Volume` | `double` | Volume (si étanche). |
| `SurfaceArea` | `double` | Aire de surface. |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `SetSubdivisionLevel(int)` | `void` | Définit le lissage. |
| `SetBox(...)` | `void` | Crée une boîte maillée. |
| `SetSphere(...)` | `void` | Crée une sphère maillée. |
| `SubdivideFace(int)` | `void` | Raffine une face spécifique. |
| `ExtrudeFaces(...)` | `void` | Extrude des faces. |

## Exemples de Code

### Exemple 1: Créer une Boîte Maillée
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    SubDMesh mesh = new SubDMesh();
    // Boîte 10x10x10 avec subdivisions 4x4x4
    mesh.SetBox(10, 10, 10, 4, 4, 4);
    
    btr.AppendEntity(mesh);
    tr.AddNewlyCreatedDBObject(mesh, true);
    
    tr.Commit();
}
```

### Exemple 2: Lisser un Maillage
```csharp
mesh.SetSubdivisionLevel(2); // Lisse la géométrie du maillage
```

### Exemple 3: Définir un Maillage Personnalisé (Sommets/Faces)
```csharp
Point3dCollection vertices = new Point3dCollection();
vertices.Add(new Point3d(0,0,0));
vertices.Add(new Point3d(10,0,0));
vertices.Add(new Point3d(0,10,0));
// ...

Int32Collection faceCounts = new Int32Collection();
faceCounts.Add(3); // Triangle

Int32Collection faceIndices = new Int32Collection();
faceIndices.Add(0); faceIndices.Add(1); faceIndices.Add(2); // Indices des sommets

SubDMesh mesh = new SubDMesh();
mesh.SetSubDMesh(vertices, faceIndices, faceCounts.Count);
```

### Exemple 4: Convertir en Solide
```csharp
// SubDMesh peut être converti en Solid3d s'il entoure un volume
if (mesh.Watertight)
{
    Solid3d sol = mesh.ConvertToSolid(false, false);
    // Ajouter sol à la base de données...
}
```

### Exemple 5: Conversion en Surface
```csharp
// Convertir en Surface (NURBS)
Autodesk.AutoCAD.DatabaseServices.Surface surf = mesh.ConvertToSurface(false, false);
```

### Exemple 6: Itérer les Sommets
```csharp
// Accéder directement aux sommets
// Attention : Cela peut être lourd
foreach (Point3d pt in mesh.Vertices)
{
    // ...
}
```

### Exemple 7: Plisser les Arêtes
```csharp
// Appliquer un pli pour maintenir les arêtes vives pendant le lissage
FullSubentityPath[] edges = ...; // Besoin d'identifier les arêtes
mesh.SetCrease(edges, 1.0); // Pli complet
```

### Exemple 8: Extruder une Face
```csharp
// Avancé : Extruder une face de sous-entité
FullSubentityPath facePath = ...;
mesh.ExtrudeFaces(new[] { facePath }, 5.0, 0.0);
```

## Meilleures Pratiques
1. **Vérification Étanchéité** : Vérifiez la propriété `Watertight` avant de tenter des calculs de volume ou une conversion en solide.
2. **Niveau de Détail** : Un `SubdivisionLevel` élevé augmente exponentiellement le nombre de sommets. Gardez-le bas pour les performances.
3. **Sous-entités** : La manipulation des faces/arêtes individuelles nécessite de les identifier via `GetSubentPathsAtGsMarker` ou des mécanismes de sélection similaires.

## Objets Associés
- [Solid3d](Solid3d.md)
- [PolygonMesh](../Geometric/PolygonMesh.md) (Hérité)

## Références
- [Référence Autodesk SubDMesh](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_SubDMesh)
