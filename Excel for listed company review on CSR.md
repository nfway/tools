## Delete sheets include some word

from :https://www.extendoffice.com/documents/excel/4092-excel-delete-sheet-if-name-contains.html#a1

1. Hold down the ALT + F11 keys to open the Microsoft Visual Basic for Applications window.

2. Click Insert > Module, and paste the following code in the Module Window.

```VBA code

Sub Deletebyname()
'Updateby Extendoffice 20160930
    Dim shName As String
    Dim xName As String
    Dim xWs As Worksheet
    Dim cnt As Integer
    shName = Application.InputBox("Enter the specific text:", "Kutools for Excel", _
                                    ThisWorkbook.ActiveSheet.Name, , , , , 2)
    If shName = "" Then Exit Sub
    xName = "*" & shName & "*"
'    MsgBox xName
    Application.DisplayAlerts = False
    cnt = 0
    For Each xWs In ThisWorkbook.Sheets
        If xWs.Name Like xName Then
            xWs.Delete
            cnt = cnt + 1
        End If
    Next xWs
    Application.DisplayAlerts = True
    MsgBox "Have deleted" & cnt & "worksheets", vbInformation, "Kutools for Excel"
End Sub

```

3.hen press F5 key to run this code, and a prompt box is popped out to remind you enter the specific text that you want to delete sheet tab based on, put it,and enter.

4.DONE

### get sheet name

```
Sub SheetNames()
Columns(1).Insert
For i = 1 To Sheets.Count
Cells(i, 1) = Sheets(i).Name
Next i
```

### 汇总多个工作表同一单元格值成一列

[来源](https://c.m.163.com/news/a/DQRFQ4OI0524G35L.html?spss=newsapp)

1、鼠标放在第一个超市名称的单元格A2，【公式】——【定义名称】：输入名称BM（此名称可任意取），引用位置处输入公式：

```
=INDEX(GET.WORKBOOK(1),ROW(A2))
```

[![AnyConv com__12](https://user-images.githubusercontent.com/16678210/87372765-ec768280-c5ba-11ea-8efd-fda2886f283a.jpg)](https://user-images.githubusercontent.com/16678210/87372765-ec768280-c5ba-11ea-8efd-fda2886f283a.jpg)

GET.WORKBOOK(1)是宏表函数，取所有工作表的名称。

2、在A2单元格输入公式：

```
=IFERROR(BM,"")
```

向下填充

3、在B2单元格输入公式：

```
=IFERROR(INDIRECT(A2&"!B1"),"")
```

公式向下填充，即得所有超市工作表B1单元格的数值：

[![AnyConv com__nimg ws 126 net](https://user-images.githubusercontent.com/16678210/87372890-35c6d200-c5bb-11ea-9626-9e925e4485fe.jpg)](https://user-images.githubusercontent.com/16678210/87372890-35c6d200-c5bb-11ea-9626-9e925e4485fe.jpg)

4、如果不喜欢上图中带工作簿名称的超市名，可以把公式改为：
`=IFERROR(MID(BM,13,9),"")`

[![nimg ws 126 net](https://user-images.githubusercontent.com/16678210/87372961-58f18180-c5bb-11ea-82a2-8a08c0f3d4d0.png)](https://user-images.githubusercontent.com/16678210/87372961-58f18180-c5bb-11ea-82a2-8a08c0f3d4d0.png)

因为工作簿名称有12个字节，所有用公式MID(BM,13,9)，从第13个字节开始提取超市名称。其中9是随意取的长度，根据超市名称字符数的多少，该数值可灵活改变。

※特别注意：

工作表名称无规律的情况，因为引用了宏表函数，所以文件保存时要保存成“启用宏的工作簿.xlsm”。

### CSV to XLS

```
Sub CSVtoXLS()
'UpdatebyExtendoffice20170814
    Dim xFd As FileDialog
    Dim xSPath As String
    Dim xCSVFile As String
    Dim xWsheet As String
    Application.DisplayAlerts = False
    Application.StatusBar = True
    xWsheet = ActiveWorkbook.Name
    Set xFd = Application.FileDialog(msoFileDialogFolderPicker)
    xFd.Title = "Select a folder:"
    If xFd.Show = -1 Then
        xSPath = xFd.SelectedItems(1)
    Else
        Exit Sub
    End If
    If Right(xSPath, 1) <> "\" Then xSPath = xSPath + "\"
    xCSVFile = Dir(xSPath & "*.csv")
    Do While xCSVFile <> ""
        Application.StatusBar = "Converting: " & xCSVFile
        Workbooks.Open Filename:=xSPath & xCSVFile
        ActiveWorkbook.SaveAs Replace(xSPath & xCSVFile, ".csv", ".xls", vbTextCompare), xlNormal
        ActiveWorkbook.Close
        Windows(xWsheet).Activate
        xCSVFile = Dir
    Loop
    Application.StatusBar = False
    Application.DisplayAlerts = True
End Sub
```

### [delete all columns except chosen one](https://stackoverflow.com/questions/50515150/delete-all-columns-except-those-named-in-all-worksheets)

```
Sub DeleteSelectedColumns()

    Dim ws As Worksheet
    Dim rDel As Range
    Dim HeaderCell As Range
    Dim sKeepHeaders As String
    Dim sDelimiter as String

    sDelmiter = ":"
    sKeepHeaders = Join(Array("2019/12/31", "2018/12/31", "2017/12/31", "2016/12/31", "2015/12/31", "2014/12/31", "2013/12/31", "2012/12/31", "2011/12/31","报告日期"), sDelimiter)

    For Each ws In ActiveWorkbook.Sheets
        Set rDel = Nothing
        For Each HeaderCell In ws.Range("A1", ws.Cells(1, ws.Columns.Count).End(xlToLeft)).Cells
            If InStr(1, sDelimiter & sKeepHeaders & sDelimiter, sDelimiter & HeaderCell.Value & sDelimiter, vbTextCompare) = 0 Then
                If Not rDel Is Nothing Then Set rDel = Union(rDel, HeaderCell) Else Set rDel = HeaderCell
            End If
        Next HeaderCell
        If Not rDel Is Nothing Then rDel.EntireColumn.Delete
    Next ws

End Sub
```

**sheet1 在操作前需隐藏**

### Excel催化剂

https://www.cnblogs.com/ExcelCuiHuaJi/p/11982610.html

### 易用宝

http://yyb.excelhome.net/features/t029/

### 工作表合并

```
Sub 工作薄间工作表合并()
Dim FileOpen
Dim X As Integer
Application.ScreenUpdating = False
FileOpen = Application.GetOpenFilename(FileFilter:="Microsoft Excel文件(.xlsx),.xlsx", MultiSelect:=True, Title:="合并工作薄")
X = 1
While X <= UBound(FileOpen)
Workbooks.Open Filename:=FileOpen(X)
Sheets().Move After:=ThisWorkbook.Sheets(ThisWorkbook.Sheets.Count)
X = X + 1
Wend
ExitHandler:
Application.ScreenUpdating = True
Exit Sub
errhadler:
MsgBox Err.Description
End Sub
```