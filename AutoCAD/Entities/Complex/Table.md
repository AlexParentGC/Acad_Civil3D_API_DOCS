# Table

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `Table` class represents a table entity in AutoCAD, similar to tables in word processors or spreadsheets. Tables can contain text, blocks, and formulas, and support merged cells and custom formatting.

**Key Concept:** Tables are intelligent entities that can automatically update, calculate formulas, and link to external data sources.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Table
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `NumRows` | `int` | Gets the number of rows |
| `NumColumns` | `int` | Gets the number of columns |
| `TableStyleId` | `ObjectId` | Gets/sets the table style |
| `FlowDirection` | `FlowDirection` | Gets/sets text flow direction |
| `HorzCellMargin` | `double` | Gets/sets horizontal cell margin |
| `VertCellMargin` | `double` | Gets/sets vertical cell margin |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `InsertRows(int, double, int)` | `void` | Inserts rows at index |
| `DeleteRows(int, int)` | `void` | Deletes rows |
| `InsertColumns(int, double, int)` | `void` | Inserts columns |
| `DeleteColumns(int, int)` | `void` | Deletes columns |
| `SetTextString(int, int, string)` | `void` | Sets cell text |
| `GetTextString(int, int, FormatOption)` | `string` | Gets cell text |
| `SetCellAlignment(int, int, CellAlignment)` | `void` | Sets cell alignment |
| `MergeCells(CellRange)` | `void` | Merges cells |
| `UnmergeCells(CellRange)` | `void` | Unmerges cells |

## Common Usage Patterns

### 1. Creating a Simple Table

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Geometry;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("CREATETABLE")]
public void CreateSimpleTable()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, 
            OpenMode.ForWrite) as BlockTableRecord;
        
        // Create table (5 rows, 3 columns)
        Table table = new Table();
        table.SetDatabaseDefaults();
        table.TableStyle = db.Tablestyle;
        table.NumRows = 5;
        table.NumColumns = 3;
        table.SetRowHeight(3.0);
        table.SetColumnWidth(15.0);
        table.Position = new Point3d(0, 0, 0);
        
        // Set header text
        table.SetTextString(0, 0, "Part Number");
        table.SetTextString(0, 1, "Description");
        table.SetTextString(0, 2, "Quantity");
        
        // Set data
        table.SetTextString(1, 0, "P-001");
        table.SetTextString(1, 1, "Bolt");
        table.SetTextString(1, 2, "100");
        
        table.SetTextString(2, 0, "P-002");
        table.SetTextString(2, 1, "Nut");
        table.SetTextString(2, 2, "100");
        
        table.GenerateLayout();
        
        btr.AppendEntity(table);
        tr.AddNewlyCreatedDBObject(table, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nTable created");
    }
}
```

### 2. Reading Table Data

```csharp
[CommandMethod("READTABLE")]
public void ReadTableData()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select table
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect table: ");
    peo.SetRejectMessage("\nMust be a table.");
    peo.AddAllowedClass(typeof(Table), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Table table = tr.GetObject(per.ObjectId, OpenMode.ForRead) as Table;
        
        ed.WriteMessage($"\n=== Table Data ({table.NumRows} rows x {table.NumColumns} cols) ===");
        
        for (int row = 0; row < table.NumRows; row++)
        {
            ed.WriteMessage("\n");
            for (int col = 0; col < table.NumColumns; col++)
            {
                string text = table.GetTextString(row, col, FormatOption.IgnoreMtextFormat);
                ed.WriteMessage($"{text}\t");
            }
        }
        
        tr.Commit();
    }
}
```

### 3. Modifying Table Content

```csharp
[CommandMethod("MODIFYTABLE")]
public void ModifyTableContent()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select table
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect table: ");
    peo.SetRejectMessage("\nMust be a table.");
    peo.AddAllowedClass(typeof(Table), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Table table = tr.GetObject(per.ObjectId, OpenMode.ForWrite) as Table;
        
        // Add new row
        table.InsertRows(table.NumRows, table.GetRowHeight(0), 1);
        
        int newRow = table.NumRows - 1;
        table.SetTextString(newRow, 0, "P-003");
        table.SetTextString(newRow, 1, "Washer");
        table.SetTextString(newRow, 2, "200");
        
        table.GenerateLayout();
        
        tr.Commit();
        ed.WriteMessage("\nRow added to table");
    }
}
```

### 4. Formatting Table Cells

```csharp
[CommandMethod("FORMATTABLE")]
public void FormatTableCells()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, 
            OpenMode.ForWrite) as BlockTableRecord;
        
        Table table = new Table();
        table.SetDatabaseDefaults();
        table.TableStyle = db.Tablestyle;
        table.NumRows = 3;
        table.NumColumns = 2;
        table.SetRowHeight(5.0);
        table.SetColumnWidth(20.0);
        table.Position = new Point3d(0, 0, 0);
        
        // Set content
        table.SetTextString(0, 0, "Header 1");
        table.SetTextString(0, 1, "Header 2");
        table.SetTextString(1, 0, "Data 1");
        table.SetTextString(1, 1, "Data 2");
        
        // Format header row
        table.SetCellAlignment(0, 0, CellAlignment.MiddleCenter);
        table.SetCellAlignment(0, 1, CellAlignment.MiddleCenter);
        table.SetTextHeight(0, 0, 4.0);
        table.SetTextHeight(0, 1, 4.0);
        
        // Set cell colors
        table.SetBackgroundColor(0, 0, Color.FromColorIndex(ColorMethod.ByAci, 7)); // White
        table.SetBackgroundColor(0, 1, Color.FromColorIndex(ColorMethod.ByAci, 7));
        
        table.GenerateLayout();
        
        btr.AppendEntity(table);
        tr.AddNewlyCreatedDBObject(table, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nFormatted table created");
    }
}
```

### 5. Merging Cells

```csharp
[CommandMethod("MERGECELLS")]
public void MergeTableCells()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, 
            OpenMode.ForWrite) as BlockTableRecord;
        
        Table table = new Table();
        table.SetDatabaseDefaults();
        table.TableStyle = db.Tablestyle;
        table.NumRows = 4;
        table.NumColumns = 3;
        table.SetRowHeight(5.0);
        table.SetColumnWidth(15.0);
        table.Position = new Point3d(0, 0, 0);
        
        // Set title (will merge across all columns)
        table.SetTextString(0, 0, "PARTS LIST");
        
        // Merge title cells
        CellRange titleRange = CellRange.Create(table, 0, 0, 0, 2);
        table.MergeCells(titleRange);
        table.SetCellAlignment(0, 0, CellAlignment.MiddleCenter);
        table.SetTextHeight(0, 0, 5.0);
        
        // Set headers
        table.SetTextString(1, 0, "Part #");
        table.SetTextString(1, 1, "Description");
        table.SetTextString(1, 2, "Qty");
        
        table.GenerateLayout();
        
        btr.AppendEntity(table);
        tr.AddNewlyCreatedDBObject(table, true);
        
        tr.Commit();
        doc.Editor.WriteMessage("\nTable with merged cells created");
    }
}
```

## Best Practices

1. **Call GenerateLayout**: Always call GenerateLayout() after modifications
2. **Row/Column Indices**: Remember indices are 0-based
3. **Text Height**: Set appropriate text heights for readability
4. **Cell Margins**: Use margins for better appearance
5. **Table Styles**: Use table styles for consistent formatting
6. **Merge Carefully**: Verify cell ranges before merging
7. **Performance**: Batch modifications before calling GenerateLayout()

## Related Classes

- [TableStyle](../SymbolTables/TableStyle.md) - Table formatting style
- [CellRange](CellRange.md) - Defines range of cells
- [DBText](../Entities/Text/DBText.md) - Text in cells

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-Table)
