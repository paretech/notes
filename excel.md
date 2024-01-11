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
- [Dynamic array formulas and spilled array behavior](https://support.microsoft.com/en-au/office/dynamic-array-formulas-and-spilled-array-behavior-205c6b06-03ba-4151-89a1-87a7eb36e531)
- [ExcelJet: Filters to extract matching values](https://exceljet.net/formulas/filter-to-extract-matching-values)
- [Spill Range Operator](https://support.microsoft.com/en-us/office/spilled-range-operator-3dd5899f-bca2-4b9d-a172-3eae9ac22efd)
- A vertical spill function can become a horizontal spill function by wraping in `transpose`


## Flag if row is visible

Use `aggregate` function to determine if a row is visible or not. This is useful because [very few Excel functions are able to exclude hidden rows](https://www.sfmagazine.com/articles/2021/june/excel-calculations-based-on-visible-rows). Working with hidden rows is useful when using table filters but you want that selection to apply to some other data in, presumably, clever ways.

```
=AGGREGATE(3,5, A2)
```
