# Excel

## Conditional Highlight Rows
1. Highlight data range (typically exclude headers)
2. Conditional Highlighting->New Rule
3. Use a formula to determine which cells to format
4. From New Formatting Rule window, Place cursor in field "Format Values where this formula is true:"
5. "Format Values where this formula is true:" field, select the cell in first row in the highlighted data range and compare against desired value.
6. Remove the row anchor (i.e. $) otherwise the rule will evaluate for the entire range based off one cell. What we want is for each row to be evaluated.
7. Note that if the first row of data and the rule do not start on same row, then unexpected results may occur. 
