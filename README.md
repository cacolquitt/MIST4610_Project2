# MIST4610_Project2

Database Usage:
The database serves several scenarios, including:

General Manager of a Team: The GM can use this database to analyze player statistics, team performance, and make decisions on player contracts, trades, and roster changes.
Fantasy Football Experts: Fantasy football experts can use this database to help assess players' performance and provide recommendations for fantasy leagues.
Data Analyst of a Football Team: Analysts can track and evaluate player performance metrics for future planning and team development.
Data Model Description:
The data model includes several interconnected tables for storing information related to football teams, players, games, and more. Key entities include:

Player: Stores player details like name, position, height, weight, and stats.
Team: Represents NFL teams, with links to coaches and stadiums.
Coach: Information about the head coaches for each team.
Stadium: Details of the stadiums where games are played.
Game: Tracks the games, teams involved, and results.
Statistics: Records player performance metrics for each game.
Injury: Tracks injuries related to players and specific games.
Season: Represents different NFL seasons.
Award: Tracks awards won by players.
Division: Defines the NFL divisions.
Contract: Stores contract details of players.
Trade: Tracks player trades between teams.
Team_Record: Tracks a team's record in a given season.
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/raw/main/Data%20Model.png?raw=true)

Data Dictionary:

Coach Table:
  Coach_ID (INT) PK: Unique ID for each coach.
  Name (VARCHAR 50): Coach's name.
  Experience (INT): Number of years as a head coach.

Team Table:
  Team_ID (INT) PK: Unique ID for each team.
  Name (VARCHAR 50): Name of the team.
  City (VARCHAR 50): The city the team is based in.
  Coach (INT) FK: References Coach_ID.

Stadium Table:
  Stadium_ID (INT) PK: Unique ID for each stadium.
  Name (VARCHAR 100): Name of the stadium.
  City (VARCHAR 50): City where the stadium is located.

Player Table:
  Player_ID (INT) PK: Unique ID for each player.
  Name (VARCHAR 50): Player's name.
  Position (VARCHAR 20): Position the player plays.
  Team_ID (INT) FK: References the team the player plays for.
  Height (INT): Height in inches.
  Weight (INT): Weight in pounds.

Contract Table:
  Contract_ID (INT) PK: Unique contract ID.
  Player_ID (INT) FK: References Player_ID.
  Team_ID (INT) FK: References Team_ID.
  Start_Date (DATE): Start date of the contract.
  End_Date (DATE): End date of the contract.
  Salary (DECIMAL): Player’s salary.

Trade Table:
  Trade_ID (INT) PK: Unique trade ID.
  Player_ID (INT) FK: References Player_ID.
  From_Team (INT) FK: References the team that is trading the player.
  To_Team (INT) FK: References the team receiving the player.
  Trade_Date (DATE): Date of the trade.

Team_Record Table:
  Record_ID (INT) PK: Unique ID for each team's record.
  Team_ID (INT) FK: References Team_ID.
  Season_ID (INT) FK: References the season.
  Wins (INT): Number of wins.
  Losses (INT): Number of losses.
  Ties (INT): Number of ties.

Statistics Table:
  Stat_ID (INT) PK: Unique ID for each statistic record.
  Player_ID (INT) FK: References Player_ID.
  Game_ID (INT) FK: References Game_ID.
  Passing_Yards (INT): Total passing yards.
  Rushing_Yards (INT): Total rushing yards.
  Receiving_Yards (INT): Total receiving yards.
  Passing_Touchdowns (INT): Total passing touchdowns.
  Rushing_Touchdowns (INT): Total rushing touchdowns.
  Receiving_Touchdowns (INT): Total receiving touchdowns.
  Interceptions (INT): Total interceptions thrown.
  Fumbles (INT): Total fumbles.

Injury Table:
  Injury_ID (INT) PK: Unique injury record ID.
  Player_ID (INT) FK: References Player_ID.
  Game_ID (INT) FK: References Game_ID.
  Description (TEXT): Description of the injury.
  Recovery_Time (INT): Estimated recovery time in weeks.

Season Table:
  Season_ID (INT) PK: Unique ID for each season.
  Year (INT): The year of the season (e.g., 2023).

Award Table:
  Award_ID (INT) PK: Unique award ID.
  Player_ID (INT) FK: References Player_ID.
  Type (VARCHAR 50): Type of award (e.g., MVP, Rookie of the Year).

Division Table:
  Division_ID (INT) PK: Unique division ID.
  Name (VARCHAR 50): Name of the division (e.g., NFC East, AFC West).
  Team_Division Table:
  Team_ID (INT) FK: References Team_ID.
  Division_ID (INT) FK: References Division_ID.

Game Table:
  Game_ID (INT) PK: Unique ID for each game.
  Season_ID (INT) FK: References the season.
  Home_Team (INT) FK: References the home team (Team_ID).
  Away_Team (INT) FK: References the away team (Team_ID).
  Stadium_ID (INT) FK: References Stadium_ID.
  Game_Date (DATE): Date of the game.

Queries:

Query #1: Multi Join
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%201%20(2).png?raw=true)
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%201.png?raw=true)

Query #2: Show all the players that have a contract worth more than $200 million, that also have more than 8 wins, have not suffered an injury, and have won an MVP award.
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%202.png?raw=true)
