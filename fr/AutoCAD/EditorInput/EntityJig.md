# EntityJig Class

## Vue d'Ensemble
Classe de base pour créer des jigs d'entité personnalisés qui permettent le glissement et la modification interactifs des entités.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Méthodes Clés
- `Sampler()` - Remplacer pour définir le comportement du jig
- `Update()` - Remplacer pour mettre à jour l'entité

## Exemple de Code
```csharp
public class LineJig : EntityJig
{
    private Point3d _endPoint;
    
    public LineJig(Line line) : base(line) { }
    
    protected override SamplerStatus Sampler(JigPrompts prompts)
    {
        PromptPointResult ppr = prompts.AcquirePoint("\nSpécifier le point final : ");
        if (ppr.Status != PromptStatus.OK) return SamplerStatus.Cancel;
        _endPoint = ppr.Value;
        return SamplerStatus.OK;
    }
    
    protected override bool Update()
    {
        ((Line)Entity).EndPoint = _endPoint;
        return true;
    }
}
```

## Classes Associées
- DrawJig, Editor

## Références
- [Documentation Officielle Autodesk](https://help.autodesk.com/view/OARX/2024/ENU/)
