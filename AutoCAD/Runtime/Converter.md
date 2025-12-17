# Converter Class

## Overview
Provides methods for converting between different unit formats and string representations.

## Namespace
`Autodesk.AutoCAD.Runtime`

## Key Methods
- `DistanceToString(double, DistanceUnitFormat)` - Converts distance to string
- `AngleToString(double, AngularUnitFormat)` - Converts angle to string
- `StringToDistance(string)` - Converts string to distance
- `StringToAngle(string)` - Converts string to angle

## Code Example
```csharp
using Autodesk.AutoCAD.Runtime;

double distance = 123.456;
string distStr = Converter.DistanceToString(distance, DistanceUnitFormat.Decimal);
ed.WriteMessage($"\nDistance: {distStr}");

double angle = Math.PI / 4;
string angleStr = Converter.AngleToString(angle, AngularUnitFormat.DegreesMinutesSeconds);
ed.WriteMessage($"\nAngle: {angleStr}");
```

## Related Classes
- DistanceUnitFormat, AngularUnitFormat

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
