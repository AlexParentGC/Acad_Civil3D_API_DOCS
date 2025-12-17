# DrawJig Class

## Vue d'Ensemble
Classe de base pour créer des jigs de dessin personnalisés pour les graphiques interactifs.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Méthodes Clés
- `Sampler()` - Remplacer pour définir le comportement du jig
- `WorldDraw()` - Remplacer pour dessiner des graphiques

## Exemple de Code
```csharp
public class CustomJig : DrawJig
{
    private Point3d _point;
    
    protected override SamplerStatus Sampler(JigPrompts prompts)
    {
        PromptPointResult ppr = prompts.AcquirePoint("\nSpécifier le point : ");
        if (ppr.Status != PromptStatus.OK) return SamplerStatus.Cancel;
        _point = ppr.Value;
        return SamplerStatus.OK;
    }
    
    protected override bool WorldDraw(WorldDraw draw)
    {
        // Dessiner des graphiques personnalisés
        return true;
    }
}
```

## Classes Associées
- EntityJig, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
