# Stock Analysis Using VBA Excel


## Overview of Project

Performed analysis of different stocks and their growth over the years 2017 and 2018.

### Purpose

The purpose of this analysis is to help aid Steve and his parents in their decision to invest in stocks. The code previously used was refactored to run the processes faster on a larger dataset including both the years 2017 and 2018.

## Analysis

### Refactored Code
    
      Dim startTime As Single
      Dim endTime  As Single

      yearValue = InputBox("What year would you like to run the analysis on?")

      startTime = Timer
    
      'Format the output sheet on All Stocks Analysis worksheet
    
      Worksheets("All Stocks Analysis").Activate
    
      Range("A1").Value = "All Stocks (" + yearValue + ")"
    
      'Create a header row
    
      Cells(3, 1).Value = "Ticker"
      Cells(3, 2).Value = "Total Daily Volume"
      Cells(3, 3).Value = "Return"

      'Initialize array of all tickers
    
      Dim tickers(11) As String
    
      tickers(0) = "AY"
      tickers(1) = "CSIQ"
      tickers(2) = "DQ"
      tickers(3) = "ENPH"
      tickers(4) = "FSLR"
      tickers(5) = "HASI"
      tickers(6) = "JKS"
      tickers(7) = "RUN"
      tickers(8) = "SEDG"
      tickers(9) = "SPWR"
      tickers(10) = "TERP"
      tickers(11) = "VSLR"
    
      'Activate data worksheet
    
      Worksheets(yearValue).Activate
    
      'Get the number of rows to loop over
    
      RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
      '1a) Create a ticker Index
    
      tickerIndex = 0

      '1b) Create three output arrays
    
      Dim TickerVolumes(12) As Long
      Dim TickerStartingPrices(12) As Single
      Dim TickerEndingPrices(12) As Single
    
      ''2a) Create a for loop to initialize the tickerVolumes to zero.
    
      For i = 0 To 11
          TickerVolumes(i) = 0
        
      Next i
    
      ''2b) Loop over all the rows in the spreadsheet.
    
      Worksheets(yearValue).Activate

      For i = 2 To RowCount
        
          '3a) Increase volume for current ticker
         
          TickerVolumes(tickerIndex) = TickerVolumes(tickerIndex) + Cells(i, 8).Value
           
          '3b) Check if the current row is the first row with the selected tickerIndex.
            
          If Cells(i - 1, 1).Value <> tickers(tickerIndex) And Cells(i, 1).Value = tickers(tickerIndex) Then
              TickerStartingPrices(tickerIndex) = Cells(i, 6).Value
            
          End If
            
          '3c) check if the current row is the last row with the selected ticker
        
          If Cells(i + 1, 1).Value <> tickers(tickerIndex) And Cells(i, 1).Value = tickers(tickerIndex) Then
              TickerEndingPrices(tickerIndex) = Cells(i, 6).Value
            
          End If

              '3d Increase the tickerIndex.
            
              If Cells(i + 1, 1).Value <> tickers(tickerIndex) And Cells(i, 1) = tickers(tickerIndex) Then
                  tickerIndex = tickerIndex + 1
                
              End If
    
      Next i
    
      '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    
      For i = 0 To 11
    
          Worksheets("All Stocks Analysis").Activate
       
          Cells(4 + i, 1).Value = tickers(i)
          Cells(4 + i, 2).Value = TickerVolumes(i)
          Cells(4 + i, 3).Value = TickerEndingPrices(i) / TickerStartingPrices(i) - 1
        
     Next i
    
      'Formatting
    
      Worksheets("All Stocks Analysis").Activate
      Range("A3:C3").Font.FontStyle = "Bold"
      Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
      Range("B4:B15").NumberFormat = "#,##0"
      Range("C4:C15").NumberFormat = "0.0%"
      Columns("B").AutoFit

      datarowstart = 4
      datarowend = 15

      For i = datarowstart To datarowend
        
          If Cells(i, 3) > 0 Then
            
              Cells(i, 3).Interior.Color = vbGreen
            
          Else
        
              Cells(i, 3).Interior.Color = vbRed
            
          End If
        
      Next i
 
      endTime = Timer
      MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

### 2017 Stock Analysis

In **Figure 1.1**, shown below, is the stock analysis performed on the 2017 dataset. From this figure it is seen that many of the stocks are growing, with the exception of "TERP" which decreased over the year of 2017, with the most increase found in "DQ", "ENPH", "FSLR", and "SEDG" return. The run time to execute the analysis of stocks for the year 2017, shown below in **Figure 1.2**, was approximately 0.30 seconds which was approximately 1/4 of the time taken to perform the analysis using the previous code. 

![](Resources/VBA_Challenge_2017.png)
<div align="center"
<p><b>Figure 1.1</b></p>
</div>

![](Resources/VBA_Challenge_2017_Runtime.png)
<div align="center"
<p><b>Figure 1.2</b></p>
</div>

### 2018 Stock Analysis

In **Figure 2.1**, shown below, is the stock analysis performed on the 2018 dataset. From this figure it is seen that many of the stocks are decreasing in total daily volume, with the exception of "ENPH," which showed a large increase in 2017 and "RUN", with the most decrease found in "DQ" and "JKS". The run time to execute the analysis of stocks for the year 2018, shown below in **Figure 2.2**, was approximately 0.31 seconds which was approximately 1/5 of the time taken to perform the analysis using the previous code.

![](Resources/VBA_Challenge_2018.png)
<div align="center"
<p><b>Figure 2.1</b></p>
</div>

![](Resources/VBA_Challenge_2018_Runtime.png)
<div align="center"
<p><b>Figure 2.2</b></p>
</div>

### Summary

In 2017 there was a large overall increase in stocks which did not sustain into 2018 where there was a large overall decrease in stocks.

#### Advantages:

* Refactoring code is used to make the processes more efficient. In this case the refactored code allowed the process to take a fraction of the time.
* Refactored code makes the code look cleaner.
* Refactored code uses less memory.

#### Disadvantages:

* Refactoring code takes time to edit and adjust code to be more efficient. 
* Refactoring code may cause issues in the way the code runs.
