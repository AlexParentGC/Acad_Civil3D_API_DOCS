# Classe Dimension

## Vue d'Ensemble
La classe `Dimension` est la classe de base pour toutes les entités de cotation dans AutoCAD, incluant les cotes linéaires, alignées, orientées, angulaires, radiales et de diamètre.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Dimension
                  ├─ AlignedDimension
                  ├─ RotatedDimension
                  ├─ RadialDimension
                  ├─ DiametricDimension
                  └─ AngularDimension
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `DimensionText` | `string` | Obtient/définit le remplacement du texte de cote |
| `Measurement` | `double` | Obtient la valeur mesurée |
| `DimensionStyle` | `ObjectId` | Obtient/définit le style de cote |
| `TextPosition` | `Point3d` | Obtient/définit la position du texte |
| `TextRotation` | `double` | Obtient/définit la rotation du texte |

## Exemples de Code

### Exemple 1: Créer une Cote Alignée
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d pt1 = new Point3d(0, 0, 0);
    Point3d pt2 = new Point3d(100, 50, 0);
    Point3d dimLinePoint = new Point3d(50, 75, 0);
    
    AlignedDimension dim = new AlignedDimension(pt1, pt2, dimLinePoint, "", db.Dimstyle);
    
    btr.AppendEntity(dim);
    tr.AddNewlyCreatedDBObject(dim, true);
    
    tr.Commit();
}
```

## Objets Associés
- [Entity](../../BaseClasses/Entity.md) - Classe de base
- [DimStyleTable](../../SymbolTables/DimStyleTable.md) - Styles de cote

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Dimension)
