# SQL_Window_HackerRank
Assignment for developing SQL skill through Dibimbing Data Engineer Bootcamp

## The Report
---
📝 Instructions : Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.
## Here's my SQL : 
```
SELECT 
    CASE WHEN g.Grade >= 8 THEN s.Name ELSE NULL END AS Name,
    g.Grade, 
    s.Marks
FROM Students s JOIN Grades g ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
ORDER BY g.Grade DESC,
    CASE WHEN g.Grade < 8 THEN s.Marks ELSE NULL END ASC,
    s.Name ASC;
```

## Result
<img width="421" height="539" alt="Screenshot 2025-09-19 at 12 58 09" src="https://github.com/user-attachments/assets/a7733b5a-3f03-4675-a8a9-8bef3bb69089" />

<img width="405" height="553" alt="Screenshot 2025-09-18 at 17 57 57" src="https://github.com/user-attachments/assets/720f6f7f-86cd-463a-ab1a-8b7fc324529a" />


This result is correct based on the instructions, but still has an error - I think it's some strict rules from HackerRank Environment

## Weather Observation Station 18
---
📝 Instructions : Consider P1(a,b) and P2(c,d) to be two points on a 2D plane.
 happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
 happens to equal the minimum value in Western Longitude (LONG_W in STATION).
 happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
 happens to equal the maximum value in Western Longitude (LONG_W in STATION).
Query the Manhattan Distance between points P1 and P2 and round it to a scale of decimal places.
## Here's my SQL : 
```
SELECT CAST(
         ABS(MAX(LAT_N) - MIN(LAT_N)) 
       + ABS(MAX(LONG_W) - MIN(LONG_W))
       AS DECIMAL(10,4))
FROM STATION;
```
## Result
<img width="803" height="494" alt="Screenshot 2025-09-19 at 13 19 03" src="https://github.com/user-attachments/assets/8f45ec89-39c3-4356-b6a8-453c4dfe37b8" />

## Top Competitors
---
📝 Instructions : Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.
## Here's my SQL : 
```
WITH 
max_score AS (
    SELECT 
        c.challenge_id,
        d.score AS max_score
    FROM Challenges c 
        JOIN Difficulty d ON d.difficulty_level = c.difficulty_level
),

per_hacker AS (
    SELECT s.hacker_id, 
    COUNT(DISTINCT s.challenge_id) AS count_max_score
    FROM Submissions s 
        JOIN max_score m ON m.challenge_id = s.challenge_id AND m.max_score = s.score
    GROUP BY s.hacker_id
    HAVING COUNT(DISTINCT s.challenge_id) > 1
),

take_name AS(
    SELECT h.hacker_id, h.name, ph.count_max_score
    FROM Hackers h 
    JOIN per_hacker ph ON ph.hacker_id = h.hacker_id
)
```

## Result
<img width="795" height="592" alt="Screenshot 2025-09-19 at 14 24 10" src="https://github.com/user-attachments/assets/2ace4044-363d-46b5-b0c4-9c1718a1e844" />

<img width="450" height="479" alt="Screenshot 2025-09-19 at 14 24 26" src="https://github.com/user-attachments/assets/3cccc888-6133-4b7f-a6db-a0ff41fb62ea" />

## Ollivander's Inventory
---
📝 Instructions : Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.
Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.
## Here's my SQL : 
```
WITH
group_col AS(
    SELECT wp.age, w.power, w.coins_needed, wp.is_evil, w.code, w.id,
    MIN(w.coins_needed) OVER(PARTITION BY w.power, wp.age ) AS coins_min
    FROM Wands w 
    JOIN Wands_Property wp ON w.code = wp.code
    WHERE wp.is_evil = 0
)

SELECT id, age, coins_needed, power
FROM group_col
WHERE coins_needed = coins_min
ORDER BY power DESC, age DESC;
```

## Result

<img width="791" height="505" alt="Screenshot 2025-09-19 at 15 05 27" src="https://github.com/user-attachments/assets/84824370-95bf-48ad-a58d-df97fe0d165a" />

<img width="583" height="481" alt="Screenshot 2025-09-19 at 15 05 34" src="https://github.com/user-attachments/assets/1f9d9a17-b082-444a-a06d-afa05a31738c" />

## Contest Leaderboard
---
📝 Instructions : You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!
The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of  from your result.
## Here's my SQL : 
```
WITH
max_score AS(
    SELECT hacker_id, challenge_id,
        MAX(score) as max_score
    FROM Submissions 
    GROUP BY hacker_id, challenge_id
),

grouped AS(
    SELECT h.hacker_id, h.name, m.max_score
    FROM max_score m
    JOIN Hackers h ON h.hacker_id = m.hacker_id
    WHERE m.max_score > 0
)

SELECT hacker_id, name, 
    SUM(max_score) as total
FROM grouped
GROUP BY hacker_id, name
HAVING SUM(max_score) > 0
ORDER BY total DESC, hacker_id ASC;
```

## Result
<img width="803" height="545" alt="Screenshot 2025-09-19 at 15 35 15" src="https://github.com/user-attachments/assets/4fabfc85-7f9a-4eb9-85f3-e71efd27391a" />

<img width="677" height="475" alt="Screenshot 2025-09-19 at 15 35 22" src="https://github.com/user-attachments/assets/9a0d0f3d-38a7-47db-be12-00ed642bba58" />













