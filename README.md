# Task-1 : Data Cleaning and Preprocessing
Clean and prepare a Raw Dataset
DATASET NAME : MOBILE SALES DATA
First : Merged the Day, Month and Year column with Date Format
Second : Change Price per unit column format from numbers to currency
Third : Rename values of days from short to large like Rename value sun to sunday, mon to monday etc.
Fourth : Remove unwanted columns like customer age
Fifth : Reordered columns according to dataset
Sixth : Deleted Duplicates 
Above changes are done in Excel Power Query
Load and transform the data now
Now in excel, add column after the unit sold column ,i.e., Total Price = Price per unit * Unit Sold


This is the Power Query Editor Description : 
let
    Source = Excel.Workbook(File.Contents("C:\Users\hp\OneDrive\Desktop\Programming\Data Analysis\Task - 1\Mobile Sales Data (Before).xlsx"), null, true),
    Sheet1_Sheet = Source{[Item="Sheet1",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Sheet1_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Transaction ID", Int64.Type}, {"Day", Int64.Type}, {"Month", Int64.Type}, {"Year", Int64.Type}, {"Day Name", type text}, {"Brand", type text}, {"Units Sold", Int64.Type}, {"Price Per Unit", type number}, {"Customer Name", type text}, {"Customer Age", Int64.Type}, {"City", type text}, {"Payment Method", type text}, {"Customer Ratings", Int64.Type}, {"Mobile Model", type text}}),
    #"Merged Columns" = Table.CombineColumns(Table.TransformColumnTypes(#"Changed Type", {{"Day", type text}, {"Month", type text}, {"Year", type text}}, "en-IN"),{"Day", "Month", "Year"},Combiner.CombineTextByDelimiter("/", QuoteStyle.None),"Merged"),
    #"Changed Type1" = Table.TransformColumnTypes(#"Merged Columns",{{"Merged", type date}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type1",{{"Merged", "Date of Sale"}}),
    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns","Sat","Saturday",Replacer.ReplaceValue,{"Day Name"}),
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","Sun","Sunday",Replacer.ReplaceValue,{"Day Name"}),
    #"Replaced Value2" = Table.ReplaceValue(#"Replaced Value1","Mon","Monday",Replacer.ReplaceValue,{"Day Name"}),
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","Wed","Wednesday",Replacer.ReplaceValue,{"Day Name"}),
    #"Replaced Value4" = Table.ReplaceValue(#"Replaced Value3","Thu","Thursday",Replacer.ReplaceValue,{"Day Name"}),
    #"Replaced Value5" = Table.ReplaceValue(#"Replaced Value4","Fri","Friday",Replacer.ReplaceValue,{"Day Name"}),
    #"Replaced Value6" = Table.ReplaceValue(#"Replaced Value5","Tue","Tuesday",Replacer.ReplaceValue,{"Day Name"}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Replaced Value6",{{"Price Per Unit", Currency.Type}}),
    #"Reordered Columns" = Table.ReorderColumns(#"Changed Type2",{"Transaction ID", "Customer Name", "Customer Age", "Date of Sale", "Day Name", "Brand", "Mobile Model", "Units Sold", "Price Per Unit", "City", "Payment Method", "Customer Ratings"}),
    #"Removed Columns" = Table.RemoveColumns(#"Reordered Columns",{"Customer Age"}),
    #"Reordered Columns1" = Table.ReorderColumns(#"Removed Columns",{"Transaction ID", "Customer Name", "Date of Sale", "Day Name", "Brand", "Mobile Model", "Price Per Unit", "Units Sold", "City", "Payment Method", "Customer Ratings"}),
    #"Removed Duplicates" = Table.Distinct(#"Reordered Columns1")
in
    #"Removed Duplicates"
