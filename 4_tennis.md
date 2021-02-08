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



3. Who has the highest first serve percentage? (Just the maximum value
   in a single match.)

4. What are the unforced error percentages of the top three players
   with the most wins? (Unforced error percentage is % of points lost
   due to unforced errors. In a match, you have fields for number of
   points won by each player, and number of unforced errors for each
   field.)


*Hint:* `SUM(double_faults)` sums the contents of an entire column. For each row, to add the field values from two columns, the syntax `SELECT name, double_faults + unforced_errors` can be used.


*Special bonus hint:* To be careful about handling possible ties, consider using [rank functions](http://www.sql-tutorial.ru/en/book_rank_dense_rank_functions.html).
