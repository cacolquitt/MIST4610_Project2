# MIST4610_GroupProject

Database Usage:
There are three different main scenarios where this database can be used: 
1. General Manager of a Team
2. Fantasy Football Experts
3. Data Analyst of a Football team

   The GM of an NFL team could use this database to filter through statistics, players, and injuries to figure out which players are making key contributions to their team. They can use this data to figure out which players to pay, which ones not to pay, and who are the best players on the team.

   Fantasy Football "experts" can use this database to make their models and decide which players are contributing well and who is not. They can target the good players and avoid the bad ones and then use this to give their terrible advice to others.

   Similarly to the GM but on a lesser level an analyst can filter through this database and make decisions about their team's future. They can also just use this to check player efficiency.

Data Model Description:
This data model represents a relational database structure for managing football-related information, focusing on various entities involved in a team's season. The core entities are Player, Team, Coach, Stadium, Game, Injury, Season, Award, Division, and Statistics.

Each player is linked to a team via the Team_ID and has attributes such as name, position, height, and weight. Teams themselves are associated with a stadium and a coach, with both having their own detailed tables (Stadium and Coach), where attributes like a coach's experience and stadium's location are stored. Games are central, with each game recording the participating teams, the stadium where it was played, and the corresponding season. Injuries are linked to both players and specific games, capturing the type of injury and recovery time. The Season and Division tables organize teams into their divisions and track the yearly records. Awards and statistics provide performance data for players, with statistics tracking metrics like yards and touchdowns for each game. Overall, this design efficiently captures a wide range of football-related data and interconnections.
![sql data model (final) for project](https://github.com/user-attachments/assets/f1dd0ad2-4d7c-4ef0-9f42-3369e290dea8)

Data Dictionary:
Coach Table:
  Tells the information about the coaches. It is only current data and headcoaches so there is only one coach per team.
  Coach_ID (INT) PK
  Name (VARCHAR 50)
  Experience (INT) How long they have been HC

Team Table
  Stores information about the football teams in the database.
  Team_ID (INT) PK
  Name (VARCHAR 50)
  City (VARCHAR 50)
  Coach (INT) FK that references the coach_id number PK

Stadium Table
  Stores infromation about the football stadiums the teams play in
  Stadium_ID (INT) PK
  Name (VARCHAR 100) 
  City (VARCHAR 50)

Season Table
  Stores details about the different seasons
  Season_ID (INT) PK
  Year (INT) 

Player Table
  Stores details about the players
  Player_ID (INT) PK
  Name (VARCHAR 50)
  Position (VARCHAR 50)
  Team_ID (INT) FK refernces the teams said player plays for
  Height (INT) inches
  Weight (INT)

Award Table
  Tracks the awards the players have won
  Award_ID (INT) PK
  Player_ID (INT) FK to track which player wins which award
  Type (VARCHAR 50)
  Season_ID (INT) FK that says in which season a player won an award

Game Table
  Tracks games in the NFL season
  Game_ID (INT) PK
  Game_Date (DATE)
  Home_Team (INT) FK that references the team table
  Away_Team (INT) FK that references the team table
  Stadium_ID (INT) FK that references the stadium table
  Season_ID (INT) FK that references the season table

Statistics Table
  Tracks stats for the season
  Stat_ID (INT) PK
  Player_ID (INT) FK that references the player table
  Game_ID (INT) FK that references the game table
  Yards (INT) 
  Touchdowns (INT)

Division Table:
  Tracks the division names
  Division_ID (INT) PK
  Name (VARCHAR 50)

Division_Team Table
  Tracks which teams play in which divisions
  Team_ID (INT) PK/FK that references the team table
  Division_ID (INT) PK/FK taht references the division table

Injuries Table
  Tracks injuries
  Injury_ID (INT) PK
  Player_ID (INT) FK that references the player table
  Game_ID (INT) FK that references the game table
  Description (VARCHAR 255)
  Recovery_Time (INT)

![image](https://github.com/user-attachments/assets/aae5fc00-53ca-47d1-a681-144e60df67ca)

Queries:

Query #1: 
SELECT 

    p.Player_ID, p.Name AS Player_Name, p.Position, p.Height, p.Weight, 

    t.Team_ID, t.Name AS Team_Name, t.City AS Team_City, 

    c.Coach_ID, c.Name AS Coach_Name, c.Experience AS Coach_Experience,

    g.Game_ID, g.Game_Date, g.Home_Team, g.Away_Team, 

    st.Stadium_ID, st.Name AS Stadium_Name, st.City AS Stadium_City,

    s.Season_ID, s.Year AS Season_Year,

    stat.Stat_ID, stat.Yards, stat.Touchdowns, 

    inj.Injury_ID, inj.Description AS Injury_Description, inj.Recovery_Time,

    a.Award_ID, a.Type AS Award_Type

FROM 

    Player p

JOIN 

    Team t ON p.Team_ID = t.Team_ID

JOIN 

    Coach c ON t.Coach = c.Coach_ID

JOIN 

    Game g ON (g.Home_Team = t.Team_ID OR g.Away_Team = t.Team_ID)

JOIN 

    Stadium st ON g.Stadium_ID = st.Stadium_ID

JOIN 

    Season s ON g.Season_ID = s.Season_ID

LEFT JOIN 

    Statistics stat ON p.Player_ID = stat.Player_ID AND g.Game_ID = stat.Game_ID

LEFT JOIN 

    Injury inj ON p.Player_ID = inj.Player_ID AND g.Game_ID = inj.Game_ID

LEFT JOIN 

    Award a ON p.Player_ID = a.Player_ID AND s.Season_ID = a.Season_ID

ORDER BY 

    p.Player_ID, g.Game_Date;

![image](https://github.com/user-attachments/assets/d305c365-71f0-49b0-88f3-cb39b6b9098a)
![image](https://github.com/user-attachments/assets/d955e23c-85cb-41e3-a512-0d7f77965277)
![image](https://github.com/user-attachments/assets/f67e22bc-b9ca-4b2d-b150-120bf616fdc0)

Query #2:

SELECT p.Name, SUM(s.Touchdowns) AS Total_Touchdowns
FROM Player p
JOIN Statistics s ON p.Player_ID = s.Player_ID
JOIN Team t ON p.Team_ID = t.Team_ID
WHERE t.Name = 'Cincinnati Bengals'
GROUP BY p.Name
HAVING SUM(s.Touchdowns) > 1;

![image](https://github.com/user-attachments/assets/b00c4653-3863-4861-ad1d-52e4a9f6f659)

Query #3:

SELECT p.Name, SUM(s.Yards) AS Total_Yards
FROM Player p
JOIN Statistics s ON p.Player_ID = s.Player_ID
LEFT JOIN Award a ON p.Player_ID = a.Player_ID AND a.Type = 'MVP'
WHERE a.Award_ID IS NULL
GROUP BY p.Name
ORDER BY Total_Yards DESC;

![image](https://github.com/user-attachments/assets/91100656-5caa-47f3-a61d-035c4b47d531)

Query #4:

SELECT p.Name, SUM(s.Yards) AS Total_Yards, i.Description AS Injury_Description, i.Recovery_Time
FROM Player p
JOIN Injury i ON p.Player_ID = i.Player_ID
JOIN Statistics s ON p.Player_ID = s.Player_ID
WHERE s.Yards > 0
GROUP BY p.Name, i.Description, i.Recovery_Time;

![image](https://github.com/user-attachments/assets/337116be-df08-4ea0-b219-0d3fdd952702)

Query #5:

Query #6: 

SELECT T.Name AS Team_Name, C.Name AS Coach_Name, P.Name AS Player_Name, S.Touchdowns
FROM Team T
JOIN Coach C ON T.Coach = C.Coach_ID
JOIN Player P ON T.Team_ID = P.Team_ID
JOIN (
    SELECT Player_ID, MAX(Touchdowns) AS Touchdowns
    FROM Statistics
    GROUP BY Player_ID
    HAVING MAX(Touchdowns) >= 3
) S ON P.Player_ID = S.Player_ID
WHERE C.Experience > 10;


Query #7: 

SELECT *
FROM Player p
JOIN Team t ON p.Team_ID = t.Team_ID
WHERE t.Name = 'Buffalo Bills';

Queery #8:

SELECT p.Name, a.Type
FROM Player p
JOIN Award a ON p.Player_ID = a.Player_ID
WHERE a.Type = 'MVP';


Query #9: 

SELECT p.Name
FROM Player p
JOIN Team t ON p.Team_ID = t.Team_ID
JOIN Game g ON t.Team_ID = g.Home_Team
JOIN Stadium s ON g.Stadium_ID = s.Stadium_ID
WHERE s.Name = 'Arrowhead Stadium';


Query #10: 

SELECT P.Name
FROM Player P
JOIN Statistics S ON P.Player_ID = S.Player_ID
WHERE S.Touchdowns > 0;




  



