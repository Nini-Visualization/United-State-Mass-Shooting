# United-State-Mass-Shooting
Queries on US mass shooting from 2018 - 2022
<<<<<<< HEAD

s01. Mass Shooting Count per State-2018
SELECT State, COUNT(*) AS TotalShootings2018
FROM shootings_2018
GROUP BY State
ORDER BY TotalShootings2018 DESC

s02. Average Dead/Injured per month, 2018.
SELECT 
    YEAR(Date) AS Year, 
    MONTH(Date) AS Month, 
    AVG(Dead) AS AvgDead, 
    AVG(Injured) AS AvgInjured
FROM shootings_2018
GROUP BY YEAR(Date), MONTH(Date)
ORDER BY Year, Month

s03. Total Number of people affected by Mass shooting in 2018

SELECT State, SUM(Total) AS TotalAffected
FROM shootings_2018
GROUP BY State
ORDER BY TotalAffected DESC

s04. Top Description of Mass shooting, 2018.

SELECT Description, COUNT(*) AS Occurrences
FROM shootings_2018
GROUP BY Description
ORDER BY Occurrences DESC

s05. Number of incident with child or teen 
SELECT COUNT(*) AS No_Child_Teen_Affected
FROM shootings_2018
WHERE Description LIKE '%child%' OR Description LIKE '%teen%';

the data created had no normalization. so tyhe best way to create a relationship between them was to use the union command.

s06. Unionized Tables 
CREATE TABLE CombinedMassShootings (
    Date DATE,
    State NVARCHAR(255),
    Dead INT,
    Injured INT,
    Total INT,
    Description NVARCHAR(MAX)
);

-- Insert data from each year's table into the combined table
INSERT INTO CombinedMassShootings (Date, State, Dead, Injured, Total, Description)
SELECT Date, State, Dead, Injured, Total, Description FROM Shootings_2018
UNION ALL
SELECT Date, State, Dead, Injured, Total, Description FROM Shootings_2019
UNION ALL
SELECT Date, State, Dead, Injured, Total, Description FROM Shootings_2020
UNION ALL
SELECT Date, State, Dead, Injured, Total, Description FROM Shootings_2021
UNION ALL
SELECT Date, State, Dead, Injured, Total, Description FROM Shootings_2022;

-- Query the combined data

s07. Total Number of massshooting in eaxh state from 2018-2022.

select State, COUNT(*) AS TotalShootings
from CombinedMassShootings
GROUP BY State
ORDER BY TotalShootings DESC

s08. Total Number of Mass shooting per year
select YEAR(Date) AS Year, COUNT(*) AS TotalShootings
from CombinedMassShootings
GROUP BY YEAR(Date)
ORDER BY Year

s09. Total Number of people killed and injured from 2018 to 2022
select SUM(Dead) AS TotalDead, SUM(Injured) AS TotalInjured
from CombinedMassShootings;

s10. The Exact Date with the highest number of mass shooting
select Date, COUNT(*) AS TotalShootings
from CombinedMassShootings
GROUP BY Date
ORDER BY TotalShootings DESC

s11. The Average Dead/Injured per every shooting in USA from 2018 to 2022

SELECT AVG(Dead) AS AvgDead, AVG(Injured) AS AvgInjured
FROM CombinedMassShootings;

s12. Top 10 mass shooting with highest number of casualty from 2018 to 2022
SELECT TOP 10 *
FROM CombinedMassShootings
ORDER BY (Dead + Injured) DESC

s13. Mass Shooting with the highest number of dead
SELECT *
FROM CombinedMassShootings
WHERE Dead = (SELECT MAX(Dead) FROM CombinedMassShootings)

s14. Mass Shooting with the highest number of Injuries.
SELECT *
FROM CombinedMassShootings
WHERE Injured = (SELECT MAX(Injured) FROM CombinedMassShootings)

s15. Months with the highest number of mass shooting
SELECT
    YEAR(Date) AS Year,
    MONTH(Date) AS Month,
    COUNT(*) AS TotalShootings
FROM CombinedMassShootings
GROUP BY YEAR(Date), MONTH(Date)
ORDER BY TotalShootings DESC

s16. Percentage mass shootings with no injuries
SELECT
    (COUNT(*) - COUNT(CASE WHEN Injured > 0 THEN 1 ELSE NULL END)) * 100.0 / COUNT(*) AS PercentageWithNoInjuries
FROM CombinedMassShootings




=======
>>>>>>> 89753cf86fe5e5fb699026b718a3358dae0f9bb9
