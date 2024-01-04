# Excel

## Working with Tables

### Resources
- [Using structured references with Excel tables (MS)](https://support.microsoft.com/en-us/office/using-structured-references-with-excel-tables-f5ed2452-2337-4f71-bed3-c8ae6d2b276e)

## Conditional Highlight Rows
1. Highlight data range (typically exclude headers)
2. Conditional Highlighting->New Rule
3. Use a formula to determine which cells to format
4. From New Formatting Rule window, Place cursor in field "Format Values where this formula is true:"
5. "Format Values where this formula is true:" field, select the cell in first row in the highlighted data range and compare against desired value.
6. Remove the row anchor (i.e. $) otherwise the rule will evaluate for the entire range based off one cell. What we want is for each row to be evaluated.
7. Note that if the first row of data and the rule do not start on same row, then unexpected results may occur. 


## Count Number of Periods

Following snippet useful to determine how many levels deep a section header might be referring two. 

`=LEN(B57)-LEN(SUBSTITUTE(B57, ".", ""))`


## Repeat Character *n* Times

Following snippet useful when trying to indent text my number of levels.

`=CONCAT(REPT("    ", I56), B56)`


## Filter on list of values

Sometimes when working with string data in Excel (e.g. requirements documents) it is useful to be able to provide a list of strings on which to filter on. The following snippet does just that!

In the following snippet, `requirements` and `SearchFor` are excel table identifiers, and `ID` is a table heading in each table.

`=FILTER(requirements, COUNTIF(SearchFor[ID], requirements[Id]), "No Result")`

Resources:
- https://support.microsoft.com/en-au/office/filter-function
