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

  



