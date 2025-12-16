# Classe Hatch

## Vue d'Ensemble
La classe `Hatch` représente une zone remplie avec un motif ou un remplissage solide dans AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Hatch
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `PatternName` | `string` | Obtient/définit le nom du motif de hachures |
| `PatternScale` | `double` | Obtient/définit l'échelle du motif |
| `PatternAngle` | `double` | Obtient/définit l'angle du motif |
| `NumberOfLoops` | `int` | Obtient le nombre de boucles de contour |
| `Associative` | `bool` | Obtient/définit si les hachures sont associatives |
| `HatchStyle` | `HatchStyle` | Obtient/définit le style de hachures |
| `Area` | `double` | Obtient l'aire des hachures |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `AppendLoop(HatchLoopTypes, ObjectIdCollection)` | `void` | Ajoute une boucle de contour |
| `SetHatchPattern(HatchPatternType, string)` | `void` | Définit le motif de hachures |
| `EvaluateHatch(bool)` | `void` | Évalue/régénère les hachures |

## Exemples de Code

### Exemple 1: Créer des Hachures Solides
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Créer un cercle à hachurer
    Circle circle = new Circle(new Point3d(100, 100, 0), Vector3d.ZAxis, 50);
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    // Créer hachures
    Hatch hatch = new Hatch();
    btr.AppendEntity(hatch);
    tr.AddNewlyCreatedDBObject(hatch, true);
    
    // Définir en remplissage solide
    hatch.SetHatchPattern(HatchPatternType.PreDefined, "SOLID");
    
    // Ajouter contour
    ObjectIdCollection boundaryIds = new ObjectIdCollection();
    boundaryIds.Add(circle.ObjectId);
    hatch.AppendLoop(HatchLoopTypes.Default, boundaryIds);
    
    // Évaluer
    hatch.EvaluateHatch(true);
    
    tr.Commit();
}
```

## Objets Associés
- [Entity](../../BaseClasses/Entity.md) - Classe de base
- [Polyline](../Polylines/Polyline.md) - Type de contour commun

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Hatch)
