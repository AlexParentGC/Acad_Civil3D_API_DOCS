# DrawJig Class

## Overview
Base class for creating custom drawing jigs for interactive graphics.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Methods
- `Sampler()` - Override to define jig behavior
- `WorldDraw()` - Override to draw graphics

## Code Example
```csharp
public class CustomJig : DrawJig
{
    private Point3d _point;
    
    protected override SamplerStatus Sampler(JigPrompts prompts)
    {
        PromptPointResult ppr = prompts.AcquirePoint("\nSpecify point: ");
        if (ppr.Status != PromptStatus.OK) return SamplerStatus.Cancel;
        _point = ppr.Value;
        return SamplerStatus.OK;
    }
    
    protected override bool WorldDraw(WorldDraw draw)
    {
        // Draw custom graphics
        return true;
    }
}
```

## Related Classes
- EntityJig, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
