# EntityJig Class

## Overview
Base class for creating custom entity jigs that allow interactive dragging and modification of entities.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Methods
- `Sampler()` - Override to define jig behavior
- `Update()` - Override to update entity

## Code Example
```csharp
public class LineJig : EntityJig
{
    private Point3d _endPoint;
    
    public LineJig(Line line) : base(line) { }
    
    protected override SamplerStatus Sampler(JigPrompts prompts)
    {
        PromptPointResult ppr = prompts.AcquirePoint("\nSpecify end point: ");
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

## Related Classes
- DrawJig, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
