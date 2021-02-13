# Part 4: Tennis Data

*Intermediate - Advanced level SQL*

---

## The challenges!

This challenge uses only SQL queries. Please submit answers in a markdown file.

1. Using the same tennis data, find the number of matches played by
   each player in each tournament. (Remember that a player can be
   present as both player1 or player2).
   
   ``` sql
   WITH tour AS (
    SELECT 'AUS_MEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM aus_men
    
    UNION ALL 
    
    SELECT 'AUS_MEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM aus_men
    
    UNION ALL 
    
    SELECT 'AUS_WOMEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM aus_women
    
    UNION ALL 
    
    SELECT 'AUS_WOMEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM aus_women
    
    UNION ALL
    
    SELECT 'FRENCH_MEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM french_men
    
    UNION ALL 
    
    SELECT 'FRENCH_MEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM french_men
    
    UNION ALL 
    
    SELECT 'FRENCH_WOMEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM french_women
    
    UNION ALL 
    
    SELECT 'FRENCH_WOMEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM french_women
    
    UNION ALL
    
    SELECT 'US_MEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM us_men
    
    UNION ALL 
    
    SELECT 'US_MEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM us_men
    
    UNION ALL 
    
    SELECT 'US_WOMEN' AS Tournament, "Player 1" AS Player, 1 AS Match_Count
    FROM us_women
    
    UNION ALL 
    
    SELECT 'US_WOMEN' AS Tournament, "Player 2" AS Player, 1 AS Match_Count
    FROM us_women
    
    UNION ALL
    
    SELECT 'WIMBLEDON_MEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM wimbledon_men
    
    UNION ALL 
    
    SELECT 'WIMBLEDON_MEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM wimbledon_men
    
    UNION ALL 
    
    SELECT 'WIMBLEDON_WOMEN' AS Tournament, "Player1" AS Player, 1 AS Match_Count
    FROM wimbledon_women
    
    UNION ALL 
    
    SELECT 'WIMBLEDON_WOMEN' AS Tournament, "Player2" AS Player, 1 AS Match_Count
    FROM wimbledon_women

    )

    SELECT Tournament, Player, SUM(Match_Count) as Total_Matches
    FROM tour
    GROUP BY Tournament, Player;
    ```
   
 <br>  

2. Who has played the most matches total in all of US Open, AUST Open, 
   French Open? Answer this both for men and women.
   
   ``` sql
   WITH tour AS (
    SELECT 'AUS' AS Tournament, 'MEN' AS Gender, LEFT(SPLIT_PART("Player1", ' ', 1), 1) AS FirstName, 
    SPLIT_PART("Player1", ' ', 2) AS LastName, 1 AS Match_Count
    FROM aus_men
    
    UNION ALL 
    
    SELECT 'AUS' AS Tournament, 'MEN' AS Gender, LEFT(SPLIT_PART("Player2", ' ', 1), 1) AS FirstName, 
    SPLIT_PART("Player2", ' ', 2) AS LastName, 1 AS Match_Count
    FROM aus_men
    
    UNION ALL 
    
    SELECT 'AUS' AS Tournament, 'WOMEN' AS Gender, LEFT(SPLIT_PART("Player1", ' ', 1), 1) AS FirstName,
    SPLIT_PART("Player1", ' ', 2) AS LastName, 1 AS Match_Count
    FROM aus_women
    
    UNION ALL 
    
    SELECT 'AUS' AS Tournament, 'WOMEN' AS Gender, LEFT(SPLIT_PART("Player2", ' ', 1), 1) AS FirstName,
    SPLIT_PART("Player2", ' ', 2) AS LastName, 1 AS Match_Count
    FROM aus_women
    
    UNION ALL
    
    SELECT 'FRENCH' AS Tournament, 'MEN' AS Gender, LEFT(SPLIT_PART("Player1", ' ', 1), 1) AS
    FirstName, SPLIT_PART("Player1", ' ', 2) AS LastName, 1 AS Match_Count
    FROM french_men
    
    UNION ALL 
    
    SELECT 'FRENCH' AS Tournament, 'MEN' AS Gender, LEFT(SPLIT_PART("Player2", ' ', 1), 1) AS
    FirstName, SPLIT_PART("Player2", ' ', 2) AS LastName, 1 AS Match_Count
    FROM french_men
    
    UNION ALL 
    
    SELECT 'FRENCH' AS Tournament, 'WOMEN' AS Gender, LEFT(SPLIT_PART("Player1", ' ', 1), 1) AS 
    FirstName, SPLIT_PART("Player1", ' ', 2) AS LastName, 1 AS Match_Count
    FROM french_women
    
    UNION ALL 
    
    SELECT 'FRENCH' AS Tournament, 'WOMEN' AS Gender, LEFT(SPLIT_PART("Player2", ' ', 1), 1) AS 
    FirstName, SPLIT_PART("Player2", ' ', 2) AS LastName, 1 AS Match_Count
    FROM french_women
    
    UNION ALL
    
    SELECT 'US' AS Tournament, 'MEN' AS Gender, LEFT(SPLIT_PART("Player1", ' ', 1), 1) AS FirstName,
    SPLIT_PART("Player1", ' ', 2) AS LastName, 1 AS Match_Count
    FROM us_men
    
    UNION ALL 
    
    SELECT 'US' AS Tournament, 'MEN' AS Gender, LEFT(SPLIT_PART("Player2", ' ', 1), 1) AS FirstName,
    SPLIT_PART("Player2", ' ', 2) AS LastName, 1 AS Match_Count
    FROM us_men
    
    UNION ALL 
    
    SELECT 'US' AS Tournament, 'WOMEN' AS Gender, SPLIT_PART("Player 1", ' ', 1) AS FirstName,
    SPLIT_PART("Player 1", ' ', 2) AS LastName, 1 AS Match_Count
    FROM us_women
    
    UNION ALL 
    
    SELECT 'US' AS Tournament, 'WOMEN' AS Gender, SPLIT_PART("Player 2", ' ', 1) AS FirstName,
    SPLIT_PART("Player 2", ' ', 2) AS LastName, 1 AS Match_Count
    FROM us_women
    
    )

    , Matches AS (
    SELECT Gender, FirstName, LastName, SUM(Match_Count) as Total_Matches, 
    RANK() OVER (PARTITION BY Gender ORDER BY SUM(Match_Count) DESC) AS Rank
    FROM tour
    GROUP BY Gender, FirstName, LastName
    ORDER BY SUM(Match_Count) DESC
    )

    SELECT Gender, FirstName, LastName, Total_Matches
    FROM Matches
    WHERE Rank = 1;
    ```

    Answer: Roger Nadal, with 21, and Victoria Azarenka, with 18, played the most matches. 

<br>

3. Who has the highest first serve percentage? (Just the maximum value
   in a single match.)

   ```sql
   WITH tour AS (
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM aus_men
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM aus_men
    
    UNION ALL 
    
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM aus_women
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM aus_women
    
    UNION ALL
    
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM french_men
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM french_men
    
    UNION ALL 
    
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM french_women
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM french_women
    
    UNION ALL
    
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM us_men
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM us_men
    
    UNION ALL 
    
    SELECT "Player 1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM us_women
    
    UNION ALL 
    
    SELECT "Player 2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM us_women
    
    UNION ALL
    
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM wimbledon_men
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM wimbledon_men
    
    UNION ALL 
    
    SELECT "Player1" AS Player, "FSP.1" AS First_Serve_Percentage
    FROM wimbledon_women
    
    UNION ALL 
    
    SELECT "Player2" AS Player, "FSP.2" AS First_Serve_Percentage
    FROM wimbledon_women

    )

    SELECT DISTINCT tour.Player, First_Serve_Percentage
    FROM tour
    INNER JOIN 
    (SELECT Player, MAX(First_Serve_Percentage) OVER () AS Max_FSP
    FROM tour) maxFSP
    ON tour.First_Serve_Percentage = maxFSP.Max_FSP
    ;
    ```

    Answer: S. Errani has the highest FSP - 93%

<br>

4. What are the unforced error percentages of the top three players
   with the most wins? (Unforced error percentage is % of points lost
   due to unforced errors. In a match, you have fields for number of
   points won by each player, and number of unforced errors for each
   field.)


  ``` sql
   WITH win AS (
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", ' ', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", ' ', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", ' ', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", ' ', 2)
       END AS LastName, 
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM aus_men
    
    UNION ALL 
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", ' ', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", ' ', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", ' ', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", ' ', 2)
       END AS LastName, 
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM aus_women
    
    UNION ALL
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", ' ', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", ' ', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", ' ', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", ' ', 2)
       END AS LastName, 
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM french_men

    UNION ALL 
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", ' ', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", ' ', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", ' ', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", ' ', 2)
       END AS LastName, 
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM french_women
    
    UNION ALL 
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", ' ', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", ' ', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", ' ', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", ' ', 2)
       END AS LastName, 
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM us_men
    
    UNION ALL 
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player 1", ' ', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player 2", ' ', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player 1", ' ', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player 2", ' ', 2)
       END AS LastName, 
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM us_women
    
    UNION ALL 
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", '.', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", '.', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", '.', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", '.', 2)
       END AS LastName,   
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM wimbledon_men
    
    UNION ALL 
    
    SELECT 
    CASE WHEN "NPW.1" > "NPW.2" THEN LEFT(SPLIT_PART("Player1", '.', 1), 1)
       WHEN "NPW.1" < "NPW.2" THEN LEFT(SPLIT_PART("Player2", '.', 1), 1)
       END AS FirstName,
    CASE WHEN "NPW.1" > "NPW.2" THEN SPLIT_PART("Player1", '.', 2)
       WHEN "NPW.1" < "NPW.2" THEN SPLIT_PART("Player2", '.', 2)
       END AS LastName,   
    CASE WHEN "NPW.1" > "NPW.2" THEN "UFE.1"/("NPW.1" + "UFE.1")
       WHEN "NPW.1" < "NPW.2" THEN "UFE.2"/("NPW.2" + "UFE.2")
       END AS Unforced_Error_Percent,
    1 AS Win_Count  
    FROM wimbledon_women
    )


    SELECT FirstName, LastName, SUM(Win_Count) as Total_Wins, 
    AVG(Unforced_Error_Percent) AS Avg_Unforced_Error_Percent
    FROM win
    WHERE LastName IS NOT NULL
    GROUP BY FirstName, LastName
    ORDER BY Total_Wins DESC
    LIMIT 3;
    ```
    
| firstname | lastname | total_wins | avg_unforced_error_percent |
|:---------:|:--------:|:----------:|:--------------------------:|
| N         | Li       |         20 |         0.4213532241457907 |
| N         | Djokovic |         16 |        0.10493692909960851 |
| V         | Azarenka |         16 |         0.4123520414540084 |


Answer: N Li has an average unforced error percentage of 42.1%, N Djokovic has an average unforced error percentage of 10.5%, and V Azarenka has an average unforced error percentage of 41.2%

<br>

*Hint:* `SUM(double_faults)` sums the contents of an entire column. For each row, to add the field values from two columns, the syntax `SELECT name, double_faults + unforced_errors` can be used.


*Special bonus hint:* To be careful about handling possible ties, consider using [rank functions](http://www.sql-tutorial.ru/en/book_rank_dense_rank_functions.html).
