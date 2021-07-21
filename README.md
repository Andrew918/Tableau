# Tableau
-----------------------------------------------------------------------------------------------
Switch Weekly or Monthly views

Display (Need to apply as a filter)
 [Report TypeW/M]

Report TypeW/M (Parameter)

Data Type: String
Current Value: Weekly
Allowable values List

Weekly  displays as Weekly
Monthly displays as Monthly

-----Before adding multiple views to a container, do above first
------------------------------------------------------------------------------------------------

Time Filter - CP

[Report Date] >= [Start Date Parameter] AND [Report Date] <= [End Date Parameter]

[Report Date] >= [Start Date Parameter]-[Week Count Parameter] AND [Report Date] <= [End Date Parameter]

--------------------------------------------------------------------------------------------------------
If wanting to show the actual dates within the time window selected.

Add a Timeframe Filter Calculated Field, drag it to filter area and select 0.

IF 
    [sent_date] >= [Start Date Parameter] AND 
    [sent_date] <= [End Date Parameter] THEN 0
ELSEIF 
    [sent_date] >= [Start Date Parameter]-([End Date Parameter]-[Start Date Parameter]+1) AND 
    [sent_date] < [Start Date Parameter] THEN 1
ELSEIF 
    [sent_date] >= [Start Date Parameter] - 365 AND 
    [sent_date] <= [End Date Parameter] - 365 THEN 2
ELSEIF
    [sent_date] >= [Comparison Start Date Parameter] AND
    [sent_date] <= [Comparison End Date Parameter] THEN 3
ELSEIF
    [sent_date] >= [Fiscal YoY Comparison - Start Date] AND
    [sent_date] <= [Fiscal YoY Comparison - End Date] THEN 4
END
----------------------------------------------------------------------------------------------------------

Week Count Parameter

Data type: Integer

current value: 4

value when workbook opens current: value

display format: automatic

allowable values: list

value: 7 display as: 1 ; value: 14 display as: 2

--------------------------------------------------------------------------------------------------
Unsubscribes - CP
IF [Time Filter - CP] = TRUE THEN [Unsubscribes] END

Unsubscribe - CP - D STR
IF 
    SUM([Unsubscribes - CP]) > 1000000 
THEN
    STR(ROUND(SUM([Unsubscribes - CP])/1000000,1)) + 'M'
ELSEIF 
    SUM([Unsubscribes - CP]) > 1000
THEN
    STR(ROUND(SUM([Unsubscribes - CP])/1000,1)) + 'K'
ELSE
    STR(ROUND(SUM([Unsubscribes - CP]),0))
END
---------------------------------------------------------------------------------------------------

Time Filter - PP

CASE [Time Comparison Parameter]
    WHEN 1 THEN
        [Report Date] >= [Start Date Parameter]-([End Date Parameter]-[Start Date Parameter]+1) AND 
        [Report Date] < [Start Date Parameter]
    WHEN 2 THEN
        [Report Date] >= [Start Date Parameter] - 366 AND 
        [Report Date] <= [End Date Parameter] - 366  
    WHEN 3 THEN 
	    [Report Date] = DATE('01-01-2001')

END

SET Time Filter CP / PP to T|F, no need to put on filter
--------------------------------------------------------------------------------------------------
Unsubscribes - PP
IF [Time Filter - PP] = TRUE then [Unsubscribes] END  

Unsubscribe - Negative Icon
IIF(zn([Unsubscribe - % Change]) < 0,'▼','')

Unsubscribe - Positive Icon
IIF(zn([Unsubscribe - % Change]) > 0,'▲','')

Unsubscribe - % Change
(SUM(ZN([Unsubscribes - CP]))/SUM(ZN([Unsubscribes - PP])))-1

-----------------------------------------------------------------------------------------------

Scorecard

Open Rate
<AGG(Unique Open Rate - CP)>  
Prev: <AGG(Unique Open Rate - PP)> | <AGG(Unique Open Rate - Negative Icon)><AGG(Unique Open Rate - Positive Icon)> <AGG(Unique Open Rate - % Change)>

------------------------------------------------------------------------------------------------
Benchmark Line to trend charts

Benchmark_Open Rate (calculated field)
ATTR(
IF [New Category] = "Winback DTV PCAN ELIG" Then 0.3603
ELSEIF [New Category] = "Winback DTV PCAN INELIG" Then 0.3792
ELSEIF [category]= "Winback DTV PDIS" Then 0.5309
ELSEIF [category]= "Winback DTV Enabler 0-60" Then 0.1745
ELSEIF [category]= "Winback DTV STMS 0-60" Then 0.1786
ELSEIF [category]= "Winback DTV STMS 61+" Then 0.1691
Else 0.3684
END
)

add Benchmark_Open Rate to "... detail" under unique open rate, then right click on the Axis, edit label, computation and so on.

----------------------------------------------------------------------------------------------------------------------------


