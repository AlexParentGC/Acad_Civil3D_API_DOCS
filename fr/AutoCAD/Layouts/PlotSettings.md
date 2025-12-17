# PlotSettings

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Vue d'Ensemble

La classe `PlotSettings` définit les paramètres de traçage pour une présentation ou une configuration de traçage nommée. Elle spécifie le périphérique de traçage, la taille du papier, l'échelle et d'autres options de traçage.

## Propriétés Clés

| Propriété | Type | Description |
|-----------|------|-------------|
| `PlotConfigurationName` | `string` | Nom du périphérique de traçage |
| `CanonicalMediaName` | `string` | Nom du format de papier |
| `PlotPaperUnits` | `PlotPaperUnit` | Unités du papier (pouces/mm) |
| `PlotRotation` | `PlotRotation` | Rotation du traçage |
| `PlotCentered` | `bool` | Traçage centré |
| `PlotType` | `PlotType` | Type de traçage (Extents/Limits/Layout/etc.) |

## Exemples de Code

### Configuration des Paramètres de Traçage

```csharp
using Autodesk.AutoCAD.DatabaseServices;

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Layout layout = tr.GetObject(LayoutManager.Current.GetLayoutId(
        LayoutManager.Current.CurrentLayout), OpenMode.ForWrite) as Layout;
    
    // Définir les paramètres de traçage
    layout.SetPlotSettingsName("DWG To PDF.pc3");
    layout.SetCanonicalMediaName("ISO_A4_(210.00_x_297.00_MM)");
    layout.SetPlotPaperUnits(PlotPaperUnit.Millimeters);
    layout.SetPlotType(PlotType.Layout);
    layout.SetPlotRotation(PlotRotation.Degrees000);
    layout.SetPlotCentered(true);
    
    tr.Commit();
}
```

## Meilleures Pratiques

1. **Valider le Périphérique**: Vérifier que le périphérique de traçage existe
2. **Format de Papier**: Utiliser des noms de format canoniques
3. **Échelle**: Définir l'échelle appropriée pour le type de traçage
4. **Rotation**: Choisir la rotation appropriée pour l'orientation du papier

## Classes Associées

- [Layout](Layout.md) - Classe dérivée pour les présentations
- PlotType - Énumération de type de traçage
- PlotRotation - Énumération de rotation

## Références

- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
