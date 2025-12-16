# XData (Extended Entity Data)

## Overview
XData (Extended Entity Data) allows you to attach custom application-specific data to AutoCAD entities. This data persists with the drawing and can be used to store information for custom applications.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Key Concepts

### Application Registration
Before attaching XData, you must register your application name in the RegAppTable.

### ResultBuffer
XData is stored and retrieved using `ResultBuffer` objects, which contain typed values.

### DXF Codes
XData uses DXF (Drawing Interchange Format) codes to identify data types:
- `1001` - Application name (required first entry)
- `1000` - String
- `1040` - Double
- `1070` - 16-bit integer
- `1071` - 32-bit integer
- `1010` - 3D point
- `1011` - 3D displacement
- `1012` - 3D direction
- `1013` - 3D distance

## Code Examples

### Example 1: Registering an Application
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    string appName = "MyApp";
    
    if (!rat.Has(appName))
    {
        rat.UpgradeOpen();
        
        RegAppTableRecord ratr = new RegAppTableRecord();
        ratr.Name = appName;
        
        rat.Add(ratr);
        tr.AddNewlyCreatedDBObject(ratr, true);
        
        ed.WriteMessage($"\nRegistered application: {appName}");
    }
    
    tr.Commit();
}
```

### Example 2: Attaching XData to an Entity
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Create XData
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MyApp"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Custom Data"),
        new TypedValue((int)DxfCode.ExtendedDataReal, 123.45),
        new TypedValue((int)DxfCode.ExtendedDataInteger32, 100)
    );
    
    // Attach XData to entity
    ent.XData = rb;
    
    rb.Dispose();
    
    tr.Commit();
}
```

### Example 3: Reading XData from an Entity
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Get all XData
    ResultBuffer rb = ent.XData;
    
    if (rb != null)
    {
        foreach (TypedValue tv in rb)
        {
            ed.WriteMessage($"\nCode: {tv.TypeCode}, Value: {tv.Value}");
        }
        
        rb.Dispose();
    }
    else
    {
        ed.WriteMessage("\nNo XData attached");
    }
    
    tr.Commit();
}
```

### Example 4: Getting XData for Specific Application
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Get XData for specific application
    ResultBuffer rb = ent.GetXDataForApplication("MyApp");
    
    if (rb != null)
    {
        ed.WriteMessage("\nXData for 'MyApp':");
        
        foreach (TypedValue tv in rb)
        {
            ed.WriteMessage($"\n  Code: {tv.TypeCode}, Value: {tv.Value}");
        }
        
        rb.Dispose();
    }
    
    tr.Commit();
}
```

### Example 5: Updating XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Get existing XData
    ResultBuffer existingRb = ent.GetXDataForApplication("MyApp");
    
    // Create new XData (replaces existing for this app)
    ResultBuffer newRb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MyApp"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Updated Data"),
        new TypedValue((int)DxfCode.ExtendedDataReal, 456.78)
    );
    
    ent.XData = newRb;
    
    if (existingRb != null) existingRb.Dispose();
    newRb.Dispose();
    
    tr.Commit();
}
```

### Example 6: Removing XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // To remove XData for a specific application, set it to null
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MyApp")
    );
    
    ent.XData = rb;
    rb.Dispose();
    
    tr.Commit();
}
```

### Example 7: Storing Complex Data
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Store various data types
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.ExtendedDataRegAppName, "MyApp"),
        
        // String
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Project Name"),
        
        // Integer
        new TypedValue((int)DxfCode.ExtendedDataInteger32, 12345),
        
        // Double
        new TypedValue((int)DxfCode.ExtendedDataReal, 3.14159),
        
        // 3D Point
        new TypedValue((int)DxfCode.ExtendedDataWorldSpacePosition, new Point3d(100, 200, 0)),
        
        // Multiple values
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Status"),
        new TypedValue((int)DxfCode.ExtendedDataAsciiString, "Approved")
    );
    
    ent.XData = rb;
    rb.Dispose();
    
    tr.Commit();
}
```

### Example 8: Finding Entities with XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    List<ObjectId> entitiesWithXData = new List<ObjectId>();
    
    foreach (ObjectId objId in btr)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        ResultBuffer rb = ent.GetXDataForApplication("MyApp");
        
        if (rb != null)
        {
            entitiesWithXData.Add(objId);
            rb.Dispose();
        }
    }
    
    ed.WriteMessage($"\nFound {entitiesWithXData.Count} entities with XData for 'MyApp'");
    
    tr.Commit();
}
```

## Common DXF Codes for XData

| Code | Type | Description |
|------|------|-------------|
| `1001` | String | Application name (required) |
| `1000` | String | ASCII string |
| `1002` | String | Control string ("{" or "}") |
| `1003` | String | Layer name |
| `1004` | Binary | Binary data |
| `1005` | String | Database handle |
| `1010` | Point3d | World space position |
| `1011` | Point3d | World space displacement |
| `1012` | Point3d | World space direction |
| `1013` | Point3d | World distance |
| `1040` | Double | Real number |
| `1041` | Double | Distance |
| `1042` | Double | Scale factor |
| `1070` | Int16 | 16-bit integer |
| `1071` | Int32 | 32-bit integer |

## Best Practices

1. **Always Register Application**: Register your application name before attaching XData
2. **Dispose ResultBuffers**: Always dispose ResultBuffer objects to prevent memory leaks
3. **Check for Null**: Always check if XData exists before processing
4. **Use Meaningful Names**: Use descriptive application names
5. **Document Structure**: Document the structure of your XData for future reference
6. **Size Limits**: XData has size limits (approximately 16KB per entity)
7. **Multiple Applications**: An entity can have XData from multiple applications

## XData vs Extension Dictionary

| Feature | XData | Extension Dictionary |
|---------|-------|---------------------|
| Storage | Limited (~16KB) | Unlimited |
| Structure | Flat list | Hierarchical |
| Complexity | Simple | Complex objects |
| Performance | Fast | Slower |
| Use Case | Simple data | Complex data structures |

## Related Objects
- [Entity](Entity.md) - Base class with XData methods
- [RegAppTable](../SymbolTables/RegAppTable.md) - Application registration
- ResultBuffer - XData container
- TypedValue - Individual XData values

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [XData Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
