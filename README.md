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

