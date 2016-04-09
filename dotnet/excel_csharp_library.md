# EPPlus - Excel .NET Library 

EPPlus is an excellent library for generating open office xml based excel files (.xlsx extension). All you need is the EPPlus.dll, Download it from 

http://epplus.codeplex.com

The software is licensed under LGPL. 

Creating a basic Excel file is straightforward. Import the following


```chsarp
using OfficeOpenXml;
using System.IO;
```
Create the excel file

```chsarp
static void Main(string[] args) {

    FileInfo excelFile = new FileInfo(@"sample.xlsx");
    ExcelPackage excel = new ExcelPackage(excelFile);
   
    var worksheet = excel.Workbook.Worksheets.Add("Data");
    worksheet.View.ShowGridLines = true;
    worksheet.Cells.Style.Font.Size = 11; //Default font size for whole sheet
    worksheet.Cells.Style.Font.Name = "Calibri";
    //Add the Content 
    worksheet.Cells[1,1].Value="Hello, World!"
    excel.Save();

}

```
All attributes such as cell type are in `Style` property. `Style` could be set for a range of cells or a single cell. 

### Add DataTable to WorkSheet

Lets say you would like to add a `System.Data.DataTable` to a worksheet. A function could be like below

```csharp
static void AddDataTable(ExcelWorksheet ws,DataTable dt,int startRow,int startCol) {
    
    int row = startRow;
    int col = startCol;

    //First Insert all the cols

    foreach(DataColumn column in dt.Columns) {
        ws.Cells[row, col].Style.Font.Bold = true;
        ws.Cells[row, col].Value = column.ColumnName;
        col++;
    }

    for (int i = 0; i < dt.Rows.Count; i++) {
        col = startCol;
        row = row + 1;
        for (int j = 0; j < dt.Columns.Count; j++) {
            ws.Cells[row, col].Value = dt.Rows[i][j];
            col++;
        }
    }

}

```


As you can see in the example. I am setting the header row as bold using `Style` properties for only for the column names.

### Formula cells

Lets expand on the previous example and add a summary row to all `System.Double` columns

```csharp
 static void AddDataTable(ExcelWorksheet ws, DataTable dt, int startRow, int startCol,bool withSummary) {

    AddDataTable(ws, dt, startRow, startCol);
    if (withSummary) {
        //Add summation data
        int sumRowStart = startRow + dt.Rows.Count + 1 ; //+1 is for the column header
        int sumColStart = startCol;
        foreach(DataColumn col in dt.Columns) {
            //Do sum from the 2nd col to the end col
            if (col.DataType == Type.GetType("System.Double")) { 
                ws.Cells[sumRowStart, sumColStart].Formula = "Sum(" + ws.Cells[startRow+1, sumColStart].Address + ":" + ws.Cells[sumRowStart-1, sumColStart].Address + ")";
            }
            sumColStart++;

        }

    }

}
```

One important thing to note is you can get the address of the cell in the Excel format (Example Cell(1,1)~A1) using the attribute `Address`. Again `Address` attribute could be used for a single cell or a range of cells. 

In the example above we are using the Excel `Sum` function.








