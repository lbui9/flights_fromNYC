# Assignment 11: Relational databases

In this Assignment you will analyze data about flights that departed from New York City airports in 2013.

We have used this dataset before, but this time the flight data is stored as a table in a database along with a table of weather data.

We will need to join these two tables together so that we can investigate the following question:

> How is a flight's departure delay related to bad weather at the departure airport?


## About the database

Running the set-up chunk will create a SQLite database in the project folder called `nycflights13.sqlite`. This database contains several tables, of which we will be using two: `flights` and `weather`.

The `flights` table contains the same dataset that we used for Assignment 3: each of the 336,776 rows represents a flight that departed a New York City (NYC) airport in 2013.

The `weather` tables contains hourly weather data for the three NYC airports (which are JFK, LaGuardia, and Newark). Each row represents the weather at one of these three airports at a particular hour during the year of 2013.

The columns in the `weather` table are:

| variable(s)              | description |
| ------------------    | ----------- |
| `origin`             | The airport weather station. Will be either: `JFK`, `LGA`, or `EWA` to match one of the 3 NYC airports in the `origin` column of the `flights` table. |
| `year`, `month`, `day`, `hour`       | Columns representing the date and hour of the weather observation. |
| `temp`, `dewp`          | Temperature and dewpoint in F. |
| `wind_dir`, `wind_speed`, `wind_gust`            | Wind direction (in degrees), speed and gust speed (in mph). |
| `precip`  | Precipitation (e.g. rain, snow, etc.), in inches. |
| `pressure`    | Sea level pressure in millibars. |
| `visib`    | Visibility in miles. |
| `time_hour`      | Date and hour of the recording as a POSIXct date. **Ignore this column for this assignment.** |


## Tips and Reminders

* Big tables of data will overflow the page in this assignment. You should not need to print out any tables of data for the exercises here, but if you decide to do so anyway then make sure you reduce the size of any tables you print in your PDF to fit within the width of a page, and not to be longer than ~1 page (but it is OK if a smaller table wraps over two pages).

* You should use the `labs()` function to add appropriate titles and axis labels to all your plots in this (and future) assignments. The syntax for doing this is:

  ```r
  ggplot(...) +
    geom_FUNCTION(...) +
    labs(
      x = "Custom x-axis label",
      y = "Custom y-axis label",
      title = "Custom title"
      )
  ```

* If you don't remember how to use a function, you can look up its help page in RStudio by typing `??function_name` in the *Console* (or by Googling the function!)

* Remember to commit your work after each question.


## Exercises

**You will need to refer back to the interactive tutorial to complete these exercises.**

1. The database is stored in a file called `"nycflights13.sqlite"`. (If this file does not exist, then you need to create it by running the set-up code chunk in your `flights_database.Rmd` answer file.)

    This is an example of a SQLite database.
  
    Add a code chunk to your `flights_database.Rmd` answer file create a database connection to the `"nycflights13.sqlite"` database. This connection should be assigned to an R variable called `con`.
    
    When you run this code chunk you should see the `con` variable appear in the *Environment* tab of RStudio. It should have a data type that says *Formal class SQLiteConnection*.
    
    Commit your work when you have finished this exercise.

2. You can see a list of the tables in the database by running this command: `DBI::dbListTables(con)` (you do not need this code in your answer file, so run it in the Console instead of putting it in a code chunk).

    The `flights` dataset that we analyzed in Assignment 3 is stored in a table of the database called `flights`.
  
    In a new code chunk, create a database table (with the `tbl()` function) for the `flights` table. Assign this table to an R variable called `flights_tbl`.
    
    When you run this code chunk you should see the `flights_tbl` variable appear in the Environment with a data type description that says *List of 2*.
    
    Commit your work when you have finished this exercise.

3. Write a query that selects the `year`, `month`, `day`, `hour`, `dep_delay`, and `origin` columns from the `flights_tbl` database table. 
    
    Even though `flights_tbl` is a database table instead of a dataframe, you can do this using the `select()` function that we have used all semester! Assign this query to a new R variable called `flights_query`.
    
    When you run this code chunk you should see the `flights_query` variable appear in the Environment with a data type description that says *List of 2*.
    
    Commit your work when you have finished this exercise.
        
4. Our database query has not yet been run. To do so, R will need to translate our R code into the language most commonly used to interact with databases: SQL.

    We do not need to write any SQL ourselves (or even understand it!), but we can get R to show us the SQL statement that will be executed.
    
    In a new code chunk, use an appropriate function to display the SQL query stored in the `flights_query` variable.
    
    Commit your work when you have finished this exercise.

5. Add a code chunk and run the `flights_query` query with the `collect()` function. Assign the resulting dataframe (which should have 336,776 rows and 6 columns) to a new variable called `flights_df`.

    Note that these are the same 336,776 flights to/from New York that we analyzed in Assignment 3!
    
    Below your code, answer this question: what column tells us the airport that a flight left from?
    
    Commit your work when you have finished this exercise.

6. Create a graph showing the distribution of the `dep_delay` column in the `flights_df` variable. Make sure to add an appropriate title.
    
    Describe the shape of the distribution.
    
    Calculate summary statistics (mean, median, standard deviation, minimum and maximum) for the `dep_delay` column using the `summarize()` function. Hint: you may have to add the `na.rm = TRUE` parameter to each of the summary functions to tell them to ignore missing data in the `dep_delay` column.
    
    Note any interesting patterns about the summary statistics (make sure that you explain why this is interesting, i.e. instead of merely stating (for example), "The mean is 10.", a better answer would be something like "The mean is 10 which means that ...").
    
    Commit your work when you have finished this exercise.

7. So far we have extracted information about flights from the *flights* table. However, we are interested in the relationship between the departure delay and the weather at the originating airport.

    The database stores the weather information in a different table, which is called `weather`. Take a look back at the *About the Database* section, and answer the following questions:
    
    i. What does each row in the *weather* table represent?
    
    ii. What column contains information about the amount of rain (or other precipitation) that fell?
    
    iii. What column indicates the airport that each weather observation was made at?
    
    Commit your work when you have finished this exercise.

8. Create an R variable for the *weather* table and then write a query that selects the following columns: `origin`, `year`, `month`, `day`, `hour`, `temp`, `wind_speed`, `precip`.

    Make sure you store the query in a new variable called `weather_query` so that it can be reused. Then in a separate line of code, use `show_query()` to print out the SQL statement for this query.

    I.e. you are essentially repeating Exercises @flights_table - @show_query.
    
    You do __not__ need to `collect()` this dataset.
    
    Commit your work when you have finished this exercise.

9. We want to create a dataset that joins the flights data with the weather data. We will do this by joining the datasets selected by flights query and our weather query.

    Join the `flights_query` (left) with `weather_query` (right) using the `left_join()` function by matching the `origin`, `year`, `month`, `day`, and `hour` columns from both datasets. 
    
    Store the new query in a new variable called `joined_query`. 
    
    Write a sentence or two explaining what this code will do.
    
    Then in a separate line of code, use `show_query()` to print out the SQL statement for this query.
    
    Commit your work when you have finished this exercise.

10. Run the `joined_query` query with the `collect()` function, and store the resulting dataframe to a new variable.

    What does each row in this new dataset represent?
    
    Commit your work when you have finished this exercise.

11. Using your new joined dataframe, create 2 scatter plots of `dep_delay` (y-axis) vs (1) `precip` and (2) `wind_speed`. 

    Describe any patterns you see in the scatter plots (i.e. does it look like flights are more delayed when there is more rain?).

12. Using your graphs from the previous exercise, write a paragraph or so answering our original question of interest ("Does bad weather affect the departure delay of flights?").

    Is this what you expected? Why or why not? 
    
    You may wish to consider that this dataset only contains flights that actually departed (and not flights that were cancelled). This is called **survivorship bias** ([Wikipedia](https://en.wikipedia.org/wiki/Survivorship_bias)). How might survivorship bias be affecting the apparent relationship between bad weather and departure delay that we are seeing in our graphs? Consider this famous story about a case of survivorship bias in World War 2:
    
    > You don’t want your planes to get shot down by enemy fighters, so you armor them. But armor makes the plane heavier, and heavier planes are less maneuverable and use more fuel. Armoring the planes too much is a problem; armoring the planes too little is a problem. Somewhere in between there’s an optimum. The reason you have a team of mathematicians socked away in an apartment in New York City is to figure out where that optimum is.
    >
    > The military came to the SRG with some data they thought might be useful. When American planes came back from engagements over Europe, they were covered in bullet holes. *But the damage wasn’t uniformly distributed across the aircraft. There were more bullet holes in the fuselage, not so many in the engines...*
    >
    > But exactly how much more armor belonged on those parts of the plane? That was the answer they came to Wald for. It wasn’t the answer they got.
    > 
    > The armor, said Wald, doesn’t go where the bullet holes are. It goes where the bullet holes aren’t: on the engines.
    > 
    > Wald's insight was simply to ask: where are the missing holes? The ones that would have been all over the engine casing, if the damage had been spread equally all over the plane? Wald was pretty sure he knew. The missing bullet holes were on the missing planes. The reason planes were coming back with fewer hits to the engine is that planes that got hit in the engine weren’t coming back.
    >
    > *Excepted from __How not to be wrong__ by Jordan Ellenberg - [full excerpt](https://medium.com/@penguinpress/an-excerpt-from-how-not-to-be-wrong-by-jordan-ellenberg-664e708cfc3d)*
    
    Commit your work when you have finished this exercise.

## Submitting

To submit the main assignment (this is required whether or not you do the bonus exercise), follow the two steps below.

1.  Save, commit, and push your completed RMarkdown file so that everything is synchronized to GitHub.
    If you do this right, then you will be able to view your completed file on the GitHub website.

2.  Knit your RMarkdown document to PDF format, export (download) the PDF file from RStudio Server, and then upload it to *Assignment 11* posting on Blackboard.


## Cheatsheets

You are encouraged to review and keep the following cheatsheets handy while working on this assignment:

*   [dplyr cheatsheet][dplyr-cheatsheet]

*   [ggplot2 cheatsheet][ggplot2-cheatsheet]

*   [RStudio cheatsheet][rstudio-cheatsheet]

*   [R Markdown cheatsheet][rmarkdown-cheatsheet]

*   [R Markdown reference][rmarkdown-reference]


[dplyr-cheatsheet]:     https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf
[ggplot2-cheatsheet]:   https://github.com/rstudio/cheatsheets/raw/master/data-visualization-2.1.pdf
[rstudio-cheatsheet]:   https://github.com/rstudio/cheatsheets/raw/master/rstudio-ide.pdf
[rmarkdown-reference]:  https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf
[rmarkdown-cheatsheet]: https://github.com/rstudio/cheatsheets/raw/master/rmarkdown-2.0.pdf

