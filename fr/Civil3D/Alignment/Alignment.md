# Classe Alignment

## Vue d'Ensemble
La classe `Alignment` représente un axe horizontal dans Civil 3D, typiquement utilisé pour les axes routiers, pipeline, ou toute caractéristique linéaire. Il peut contenir des courbes, des lignes et des spirales.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Hiérarchie d'Héritage
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ Alignment
```

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `Name` | `string` | Obtient/définit le nom de l'axe |
| `Description` | `string` | Obtient/définit la description |
| `Length` | `double` | Obtient la longueur totale |
| `StartingStation` | `double` | Obtient/définit la valeur de la station de départ |
| `EndingStation` | `double` | Obtient la valeur de la station de fin |
| `SiteId` | `ObjectId` | Obtient/définit l'ObjectId du site parent |
| `StyleId` | `ObjectId` | Obtient/définit le style de l'axe |
| `Entities` | `AlignmentEntityCollection` | Obtient la collection des entités de l'axe |

## Méthodes Clés

| Méthode | Type de Retour | Description |
|---------|----------------|-------------|
| `GetProfileViewIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de profils en travers |
| `GetProfileIds()` | `ObjectIdCollection` | Obtient tous les ObjectIds de profils en long |
| `StationOffset(double, double, out double, out double)` | `void` | Convertit XY en station/décalage |
| `PointLocation(double, double)` | `Point3d` | Obtient XY depuis station/décalage |
| `GetStationAtPoint(Point3d)` | `double` | Obtient la station à un point |

## Exemples de Code

### Exemple 1: Accéder à un Axe
```csharp
using Autodesk.Civil.DatabaseServices;

using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection alignmentIds = civilDoc.GetAlignmentIds();
    
    if (alignmentIds.Count > 0)
    {
        Alignment alignment = tr.GetObject(alignmentIds[0], OpenMode.ForRead) as Alignment;
        
        ed.WriteMessage($"\nAxe : {alignment.Name}");
        ed.WriteMessage($"\nLongueur : {alignment.Length:F2}");
        ed.WriteMessage($"\nStation Début : {alignment.StartingStation:F2}");
        ed.WriteMessage($"\nStation Fin : {alignment.EndingStation:F2}");
    }
    
    tr.Commit();
}
```

### Exemple 2: Obtenir Point depuis Station/Décalage
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    double station = 100.0;
    double offset = 5.0; // 5 unités à droite
    
    Point3d point = alignment.PointLocation(station, offset);
    
    ed.WriteMessage($"\nPoint à Station {station}, Décalage {offset} :");
    ed.WriteMessage($"\n  X : {point.X:F2}, Y : {point.Y:F2}");
    
    tr.Commit();
}
```

### Exemple 3: Convertir XY en Station/Décalage
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    Point3d testPoint = new Point3d(1000, 2000, 0);
    
    double station, offset;
    alignment.StationOffset(testPoint.X, testPoint.Y, out station, out offset);
    
    ed.WriteMessage($"\nPoint ({testPoint.X:F2}, {testPoint.Y:F2}) :");
    ed.WriteMessage($"\n  Station : {station:F2}");
    ed.WriteMessage($"\n  Décalage : {offset:F2}");
    
    tr.Commit();
}
```

### Exemple 4: Itérer à Travers les Entités d'Axe
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    foreach (AlignmentEntity entity in alignment.Entities)
    {
        ed.WriteMessage($"\nType Entité : {entity.EntityType}");
        ed.WriteMessage($"\n  Station Début : {entity.StartStation:F2}");
        ed.WriteMessage($"\n  Station Fin : {entity.EndStation:F2}");
        ed.WriteMessage($"\n  Longueur : {entity.Length:F2}");
        
        if (entity.EntityType == AlignmentEntityType.Arc)
        {
            AlignmentArc arc = entity as AlignmentArc;
            ed.WriteMessage($"\n  Rayon : {arc.Radius:F2}");
        }
    }
    
    tr.Commit();
}
```

### Exemple 5: Obtenir Profils Associés
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    Alignment alignment = tr.GetObject(alignmentId, OpenMode.ForRead) as Alignment;
    
    ObjectIdCollection profileIds = alignment.GetProfileIds();
    
    ed.WriteMessage($"\nProfils pour axe '{alignment.Name}' :");
    
    foreach (ObjectId profileId in profileIds)
    {
        Profile profile = tr.GetObject(profileId, OpenMode.ForRead) as Profile;
        ed.WriteMessage($"\n  {profile.Name}");
    }
    
    tr.Commit();
}
```

## Objets Associés
- [Profile](Profile.md) - Profil vertical le long de l'axe
- [CivilDocument](../Core/CivilDocument.md) - Conteneur pour axes
- Feature - Classe de base

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Alignment)
