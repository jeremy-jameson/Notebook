# Excel Macros

Thursday, May 31, 2012
12:56 PM

```VBScript
Option Explicit

Public Function GetAddress(rng As Range) As String
    If rng.Cells.Count > 1 Then
        GetAddress = CVErr(xlErrValue)
    Else
        GetAddress = rng.Hyperlinks.Item(1).Address
    End If
End Function

Public Function XmlEncode(rng As Range) As String
    If rng.Cells.Count > 1 Then
        XmlEncode = CVErr(xlErrValue)
    Else
        Dim s As String
        s = rng.Value

        s = Replace(s, "&", "&amp;")
        s = Replace(s, "<", "&lt;")
        s = Replace(s, ">", "&gt;")
        s = Replace(s, """", "&quot;")

        XmlEncode = s
    End If
End Function
```
