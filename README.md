# Movies Extract Transform Load

## Purpose

Create an automated pipeline that takes in raw data, performs the appropriate transformation and loads the data into existing tables using Pandas, Python and ETL process. 

## Result

This analysis divided in four parts. Let’s have a look of each part.

1.	__ETL Function to Read Three Data Files__

    * Function takes three files; __Wikipedia JSON file, Kaggle metadata and MovieLens ratings CSV files__ as arguments
    * Read Kaggle metadata and MovieLens CSV files into Pandas DataFrames.
    * Open Wikipedia JSON file and convert JSON data into raw data and store that data into Pandas DataFrame.
    * Function returns three DataFrames that saves to appropriate DataFrames by calling the function.

2.	__Extract and Transform the Wikipedia Data__

    *  Define a function that __cleans__ the wiki movie objects by making a list of alternative titles and looping through that list, if current key-value pair exists        in the movie object, remove the key value pair and add to the alternative titles. 
    * Using function delated in previously read the __three  data files__. 
    * Using regular expression extract the __IMDB IDs__ and drop if any duplicate IDs are there and catch errors writing this code in __try-except__ block.
    * More data cleaning by holding columns who don’t have __null values__.
    * Define form1 __(\$\d+\.?\d*\s*[mb]illion)__ pattern which contain six elements to match;  a dollar sign, an arbitrary (but non-zero) number of digits, an               optional decimal point, an arbitrary (but possibly zero) number of more digit, a space (maybe more than one), the word “million” or “billion”. 
    * Define form2 __(\$\d{1,3}(?:,\d{3})+)__ pattern to match a dollar sign, a group of one to three digits, at t least one group starting with a comma and followed         by exactly three digits.
    * Define the function which parse the dollar sign: takes an input as string from __Box Office__ column, if input is not the string, returns __NaN__, removes             dollar sign, comma/billion/million, converts into float and returns. 
    * Using form1, form2 patterns and parse dollar function, parse the box office values to numeric values.
    * Using lambda function and parse dollar function with form1 and form2 matching pattern, clean the budget column.
    * Parse the release date dropping null values and using four matching patterns:
        1.	Full month name, one- to two-digit day, four-digit year (i.e., January 1, 2000)
        
            __?:January|February|March|April|May|June|July|August|September|October|November|December)\s[123]?\d,\s\d{4}__
            
        2.	Four-digit year, two-digit month, two-digit day, with any separator (i.e., 2000-01-01)
   
            __\d{4}.[01]\d.[0123]\d__
            
        3.	Full month name, four-digit year (i.e., January 2000)
        
            __(?:January|February|March|April|May|June|July|August|September|October|November|December)\s\d{4}__
            
        4.	Four-digit year
        
            __\d{4}__
            
    * Parse the __Running Time__ column by dropping  null values , extracting desired values using regular expression pattern 
      __(\d+)\s*ho?u?r?s?\s*(\d*)|(\d+)\s*m__         and using lambda function convert them to numeric values. 

3.	__Extract and Transform Kaggle Data__

    * Keeping rows where the adult column is __False__, and then drop the adult column.
    * Set other columns like __video, budget, id, popularity, release date__ to correct data types.
    * Convert the __timestamp__ column in Ratings Data to datetime data type.
    * Merge the Wikipedia and Kaggle metadata DataFrames on IMDB IDs.
    * Drop unwanted columns like title_wiki, release_date_wiki, Language, and Production company(s). 
    * Define a function that fills in missing data for a column pair and then drops the redundant column.
    * Reorder and rename the columns in merged DataFrame.
    * Clen the rating counts and merged cleaned ratings DataFrame with the previously merged DataFrame and fill the empty values in this DataFrame with “0”.

4.	__Create the Movie Database__

    * Create a Database engine to connect SQL and import the  __Movie Data__ and __Ratings Data__ .
    * Write a function that displays the elapsed time while importing data. 


## __Summary__

This ETL process extract data from three data sources (Wikipedia, Kaggle and MovieLens) as three files, two CSVs and a JSON raw data. After transforming them to  a merged DataFrames using different data cleaning steps and procedures, finally stored into SQL database as tables. 

Data cleaning process might differ from person to person, team to team or company to company.  One person merged cleaned DataFrame might be different form others. Finally, for good, accurate and easy to understood data reports data must be passed through this messy cleaning process. 


