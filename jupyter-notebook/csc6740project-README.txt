CSC-6740 Term Project - Fall 2020 ReadMe Notes
William Dobson

Two Jupyter notebooks were created for this project to scrape and collect web news stories, mine them for sentiment, 
analyze and compare sentiments, and produce visualizations of the data.  All dependencies should be at the top of each notebook.

A series of folders are expected under the run directory as follows:

rss/     - stores daily rss news items list files (without article text bodies)
news/    - stores lists of complete articles (text bodies scraped) with placeholders for sentiment values    
           'polarity' and 'subjectivity' used to store mining results.
snews/   - news articles list updated with sentiment values from the data mining process.
newsavg/ - stores lists of statistical data class objects used in analysis
snewsdf/ - stores pandas dataframes from the minng process
 
csc6740-rss-scraper:
----------------------
This code is used to collect lists of XML news items from RSS feeds using a hard coded list of URLs at the top of the program.  
To remove a news source just #comment out that line. The news items contain title, date, description, key(news source), link (URL) 
to the story, and placeholders for other data. 

When running the code the user will be prompted for a search term with 2 possible results:
------------------------------------------------------------------------------------------
1. If an '*' is entered the code will download all RSS items from the feeds and store (pickle) them in an rss/ folder under 
   the run directory with a filename:   rss/rss_all_yyyy-mm-dd.p  
   Note: the text body is not downloaded at this time but rather this is a way collect reference daily lists for future downloading.

2. If a search term (word) is entered this program will search the days RSS feeds for the topic and scrape the article text body 
   and reference information then store them in a news/ folder under the run directory using the filename news/news_searchterm_yyyy-mm-dd.p   

csc6740-sentiment-mining:
---------------------------
This code has the following functionality:
1. Scrapes news stories from RSS list files (one or many) and merge all the data into one data 
   file for further analysis. 
2. Reads news files with text already scraped for analysis.
3. Mines the data list for sentiment
4. Provides options for filtering the data
5. Provides many visualization options in the code

When running this code the user is prompted for:
--------------------------------------------------- 
1. A filename (Be sure to include the path in the filename):
*  If the file is an RSS list it will first scrape all the missing text bodies before proceding 
   otherwise it will work with the existing data.

2. A search term. 
*  In the case of an RSS list file you should enter a search term here to avoid attempting to scrape 
   all the news stories.
*  For previously scraped news files these are already sorted by topic so just enter '*' or just 
   hit enter to continue.  Note that it is possible to use this term as a second level search of the 
   news articles but this has not been tested.

The next code cell in the notebook is the main data mining loop that will clean the data and generate 
new lists of 'cleaned' data for text mining and an updated "save_items" list to store the updated news 
articles combined with sentiment data.  The code stores the save_items, pandas data frames, and statistics lists 
in their appropriate folders.

Data Separation Filter
-----------------------
Before exiting the main loop a filter section has been added to ensure mutual exclusivity for the lists 
when comparing two topics as we did for 'biden' and 'trump'.   The parameters for this filter are 
located at the end of the user input cell for convienience when runing the program.  
The parameters are:

1.  sterm1 - the first search term
2.  sterm2 - the second search term that should not exist in any results returned for sterm1 

3.  fmode - sets the filtering mode.  
    fmode = 0 - Bypasses the filter funtion (**USE THIS if not comparing two topics).
    fmode = 1 - Searches both title and description for each story returning those that 
                contain sterm1 but not sterm2  (most strict rule)
    fmode = 2 - Searches titles only for each story returning those that 
                contain sterm1 but not sterm2
    fmode = 3 - Searches both title and description for each story returning those that 
                contain sterm1 but not sterm2 only in the title (less strict rule)

Once processed by the filter stage "fsave_terms" is available for the subsequent visualiazion and analysis cells. 

*Also note that the common input data pathes/filenames are in the comments at the user input cell for easy cut 
and paste running the data sets.  Note that there are time series data sets for other subjects such as 
'covid', 'election', and 'china' for the same time frame.
# Available filenames:  news/news_biden_2020-10-24-to-2020-11-06.p  news/news_trump_2020-10-24-to-2020-11-06.p
#                       news/news_covid_2020-10-24-to-2020-11-06.p  news/news_election_2020-10-24-to-2020-11-06.p
#                       news/news_china_2020-10-24-to-2020-11-06.p  

Global Lists used:
------------
"datelist" -  a default datelist containing the dates from Oct 24 to Nov 06 used for time series analysis 
              that is usually copied to the name "dlist" before passed to functions so that the user can
              restrict the which dates to test.
datelist = ['2020-10-24', '2020-10-25', '2020-10-26', '2020-10-27', '2020-10-28', '2020-10-29', '2020-10-30',\
            '2020-10-31', '2020-11-01', '2020-11-02', '2020-11-03', '2020-11-04', '2020-11-05', '2020-11-06' ]

"keylist"  -  a list of news source mnemonics used for sorting an referencing data. Typically this is
              copied to to "klist" when passed to functions so the user can restrict which sources 
              are analyzed. 

keylist = ['bbc', 'cnbc', 'cnn', 'foxnews', 'nbcnews', 'npr', 'nyt', 'washpost', 'washtimes', 'wsj' ] 


Visualization functions:
------------------------
There are many visualiztion functions in this project. Most that have "mode" parameter variables will have 
these settings documented in the comments where used.





