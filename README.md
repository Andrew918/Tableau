# Tableau
-----------------------------------------------------------------------------------------------
View Dropdown tricks

Display (Need to apply as a filter)
 [Report TypeW/M]

Report TypeW/M (Parameter)

Data Type String
Current Value Weekly
Allowable values List

Weekly  displays as Weekly
Monthly displays as Monthly

-----Before adding multiple views to a container, do above first
------------------------------------------------------------------------------------------------

Time Filter - CP

[Report Date] >= [Start Date Parameter] AND [Report Date] <= [End Date Parameter]
--------------------------------------------------------------------------------------------------
Unsubscribes - CP
IF [Time Filter - CP] = TRUE THEN [Unsubscribes] END

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

--------------------------------------------------------------------------------------------------
Unsubscribes - PP
IF [Time Filter - PP] = TRUE then [Unsubscribes] END  

-----------------------------------------------------------------------------------------------
