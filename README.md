Sub AnalyzeStockData()
    ' Declare variables
    Dim Last_Row As Long
    Dim ws As Worksheet
    Dim Ticker As String
    Dim OpeningPrice As Double
    Dim ClosingPrice As Double
    Dim YearlyChange As Double
    Dim PercentageChange As Double
    Dim TotalStockVolume As Double
    Dim SummaryTable As Long
  
   ' Initialize variables for tracking max/min values for 2nd part of question
    Dim MaxPercentageIncrease As Double
    Dim MinPercentageDecrease As Double
    Dim MaxTotalVolume As Double
    Dim MaxPercentageIncreaseTicker As String
    Dim MinPercentageDecreaseTicker As String
    Dim MaxTotalVolumeTicker As String
    
        
   
   ' Loop through all worksheets
    For Each ws In Worksheets
        ' Assign column headers
        ws.Range("I1").Value = "Ticker"
        ws.Range("J1").Value = "Yearly Change"
        ws.Range("K1").Value = "Percent Change"
        ws.Range("L1").Value = "Total Stock Volume"
        ws.Range("P1").Value = "Ticker"
        ws.Range("Q1").Value = "Value"
        
        ' Find the last row with data in column A
        Last_Row = ws.Cells(ws.Rows.Count, "A").End(xlUp).Row
        
        ' Initialize variables for the summary table
        SummaryTable = 2
        Ticker = ws.Cells(2, 1).Value
        OpeningPrice = ws.Cells(2, 3).Value
        TotalStockVolume = 0
        
        ' Start looping through the data
        For i = 2 To Last_Row
            If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
                ' Set Closing Price and calculate Yearly Change and Percentage Change
                ClosingPrice = ws.Cells(i, 6).Value
                YearlyChange = ClosingPrice - OpeningPrice
                If OpeningPrice <> 0 Then
                    PercentageChange = (YearlyChange / OpeningPrice)
                Else
                    PercentageChange = 0
                End If
                
         ' Print information to the summary table
                ws.Cells(SummaryTable, 9).Value = Ticker
                ws.Cells(SummaryTable, 10).Value = YearlyChange
                ws.Cells(SummaryTable, 11).Value = PercentageChange
                ws.Cells(SummaryTable, 11).NumberFormat = "0.00%"
                ws.Cells(SummaryTable, 12).Value = TotalStockVolume
                
                 ' Check for the "Greatest % Increase," "Greatest % Decrease," and "Greatest Total Volume"
                If PercentageChange > MaxPercentageIncrease Then
                    MaxPercentageIncrease = PercentageChange
                    MaxPercentageIncreaseTicker = Ticker
                

                ElseIf PercentageChange < MinPercentageDecrease Then
                    MinPercentageDecrease = PercentageChange
                    MinPercentageDecreaseTicker = Ticker
             

                ElseIf TotalStockVolume > MaxTotalVolume Then
                    MaxTotalVolume = TotalStockVolume
                    MaxTotalVolumeTicker = Ticker
                End If
                
                 ' Conditional formatting for positive change in green and negative change in red
                If YearlyChange > 0 Then
                    ws.Cells(SummaryTable, 10).Interior.ColorIndex = 4 ' Green
                ElseIf YearlyChange < 0 Then
                    ws.Cells(SummaryTable, 10).Interior.ColorIndex = 3 ' Red
                End If
                
                ' Move to the next line in the summary table
                SummaryTable = SummaryTable + 1
                
                ' Reset variables for the new ticker
                Ticker = ws.Cells(i + 1, 1).Value
                OpeningPrice = ws.Cells(i + 1, 3).Value
                TotalStockVolume = 0
            Else
                ' Accumulate total stock volume for the current ticker
                TotalStockVolume = TotalStockVolume + ws.Cells(i, 7).Value
            End If
        Next i
        
        ' Output the "Greatest % Increase," "Greatest % Decrease," and "Greatest Total Volume" information
    ws.Cells(2, 15).Value = "Greatest % Increase"
    ws.Cells(3, 15).Value = "Greatest % Decrease"
    ws.Cells(4, 15).Value = "Greatest Total Volume"

    ws.Cells(2, 16).Value = MaxPercentageIncreaseTicker
    ws.Cells(3, 16).Value = MinPercentageDecreaseTicker
    ws.Cells(4, 16).Value = MaxTotalVolumeTicker

    ws.Cells(2, 17).Value = MaxPercentageIncrease
    ws.Cells(3, 17).Value = MinPercentageDecrease
    ws.Cells(4, 17).Value = MaxTotalVolume
    
    ws.Cells(2, 17).NumberFormat = "0.00%"
    ws.Cells(3, 17).NumberFormat = "0.00%"
    ws.Cells(4, 17).Value = MaxTotalVolume
    
   Next ws
   
    
End Sub



