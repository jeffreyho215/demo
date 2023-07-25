# demo

Hello, I am Jeffrey. I am an aspiring Marketing graduate who seeks to learn more about the world of coding and statistics. Without any numbers
it is impossible for any marketing campaigns to be proceeded. Besides, coding and statistics are essentially puzzles that we can have fun with.
Moreover, without visualizing the data, they are nothing but pointless dots on the charts, meaningless numbers on the spreadsheet. It is vital
for a marketer to succinctly and clearly demonstrate the results through charts so that a message can be relayed with no problem.

This project represents my effort to achieve this goal. The following will explain the projects goal, the methods, and the results.


------------------------------------------------------------------------------------------------------------------------------------------------------


Purpose and goal:

Economics 101 teaches us that there is a relationship between supply and demand for anything. In an ideal world, everyone provides goods and 
services that are priced in a perfectly fair way. The price tags represent totally the value of something. If the something is doing well 
according to some metrics, then the price tag should have increased numbers, and vice versa. People naturally want something that is of better
quality. Therefore the price for such goods or services should be higher than all else. If this good or service has, for whatever reason, 
experienced a decline in quality, then its demand should fall. An equilibrium of price of the good/service and quantity provided set in to 
readjust themselves constantly.Applying this logic to an athlete, the metric should therefore be his/her performance in the arena 
measured by how many wins or losses he or she has achieved (service quality), and the salary he or she receives (price).  The more wins 
he or she achieves, the greater the salary is received, in the perfect, economics 101 world. The correlation between the price and service quality
should be 1. The question therefore is: How real is this in the actual setting?

The goal of this project is to find out the answer to this question. More specifically, this project seeks to find out the correlation
between NBA players' (grouped as teams) salaries and the W/L ratio to see if the market cares about exactly how many wins a team has, therefore
reflecting this liking to the salaries, or is there some other forces  that cannot be explained by W/L ratio and need to be explained by another
project.

------------------------------------------------------------------------------------------------------------------------------------------------------


Method (source)

The source (https://hoopshype.com/salaries/players) contains the data of salaries of NBA players, from the highest salaried to the lowest. 
There is no team in the data that map the players to their teams. Other sources will be used to do so, therefore aggregating the salaries 
of a team in a year to calculate the aggregated relation with the W/L ratio. This particular URL contains only 2023-24, and forecasts into 
3 years into the future. Other URLs have data in the past. The project uses URLs that contained the data from 2015 to 2022. 

To map the players to the correct teams, https://www.basketball-reference.com/leagues/NBA_2022_totals.html  
is used (This URL only has the stats in 2021-22. The site has stats before this period back to 15-16. These sources are used in this project.
For the sake of being brief, the URls are not being listed here in this README file). In the project, I downloaded the file in the csv format to 
practice my skill in scraping data from this format. Therefore when downloading this project, please be adviced since it may not work unless the 
relevant lines are revised to be the correct file addresses.

For the W/L ratio of the team, http://www.espn.com/nba/stats/rpi  is used. This URL provides the latest available data. The website has backlogs 
for the data in the past. Both are used in this project.


------------------------------------------------------------------------------------------------------------------------------------------------------


Method (Assembling the data and cleaning)



First, I scraped from the hoopshype site by seeking the table in the html codes through the BeautifulSoup (image shown below)
![Alt text](/BeautifulSoup_scrape.png?raw=true "BeautifulSoup_scrape")
Then I seeked all <td> sections in the source codes and discarded any lines of codes that are not player names or salaries by locating the 
9th line of the <td> section to the 3008th. From the 9th line, scrape the names, then after 8 lines, scrape the 17th for another name; same
principle for the salaries. This goes on from 2015 to 2022 data. The data are then listed in 2 lists (image shown below).
![Alt text](/<td>%20find%20the%20names%20and%20salaries.png?raw=true  "names and salaries")


Dictionaries for the names and salaries are created, then a dataframe (salary_df) is created from the dictionaries. Total of 8 dataframes are
built for the 8 periods examined (image shown below)
![Alt text](/dictionaries%20and%20dataframes.png?raw=true  "dict and df")

Dollar signs and commas are removed from the dataframe. The salary columns are transformed into float 64s from strings for calculations. (image shown below)
![Alt text](/dollar%20and%20comma.png?raw=true  "dollar and comma")

Then I scraped from wikipedia to use the abbreviations of the teams. BeautifulSoup is again used, same princple as scraping from hoopshype.
Dataframe is built. The dataframe is rearranged into ascending order (first letter of each abbreviation). The index column is dropped.
![Alt text](/abbreviation.png?raw=true  "abbreviation")

Here I used the data from ESPN to first build a dataframe (stats_df) housing the player names. Then I removed the rows with TOT since it refers to the 'TOTal'
of all metrics measuring the players listed in the data. Then I matched the abbreviations dataframe with the current dataframe to match the full 
team names with abbreviations. For the salaries, I merge dataframe salary_df (for 2021-22) with stats_df (for 2021-22). Data cleaning follows.The
Franchise rows that have no value are removed. Then player names and index columns are dropped. And the salaries are rearranged with groupby(), then
added together according to the Franchise values. This repeats separately for the 8 periods.
![Alt text](/match%20dfs%20abbreviation.png?raw=true  "match_dfs_abbreviation")

The 8 periods salary dfs are merged everytime the process described in the previous paragraph for each new period
is completed. The name of this dataframe is 'new'. The method used to merge is outer, or using the Franchise column to merge the 2 
dataframes, even if there are empty or different valued cells in this column
![Alt text](/salary%20df%20merge.png?raw=true  "salary_df_merge")

Following this step, I scrape from the ESPN site for the W/L ratio. The process is the same as scraping from hoopshype. 8 dataframes
for each of the 8 periods. 
![Alt text](/W/L%20ratio.png?raw=true  "W/L_ratio")

I then merged these W/L ratio dataframes into one, using the inner method, or using Franchise column to merge, and drop the rows that
have no values or different values in the Franchise cell. The end-dataframe is named dfyear6
![Alt text](/merging%20the%20W/L%20ratio.png?raw=true  "merging_the_W/L_ratio")

The final dataframe is built by merging the salary dataframe (new6), with W/L ratio dataframe (dfyear6) with the inner method.
Franchise column is set as the index column.  Then correlations are calculated for each period's total salary of each team
with the W/L ratio of each team. 
![Alt text](/Ending%20dataframe.png?raw=true  "Ending_dataframe")

A list of correlations is built, with another list of years constructed. Correlations are rounded to 3 decimals, and plotted as 
a line chart. 
![Alt text](/Plotting.png?raw=true "Plotting")

The resulting chart
![Alt text](/Chart.png?raw=true "Chart")


------------------------------------------------------------------------------------------------------------------------------------------------------


Result


Through out the tested periods (2015-2022), correlations frequently remained low by convention. The highest correlation was 0.249, the lowest 
was minus 0.419, high by convention. 6 out of 8 periods had either low or very low correlations. Therefore this means that 
there is little relationship between performing well in the basketball field and receiving high salary. 
