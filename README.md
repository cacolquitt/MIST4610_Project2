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

SELECT 
    p.Player_ID AS PlayerID,
    p.Name AS PlayerName, -- Player Name
    p.Team_ID AS PlayerTeamID,
    t.Team_ID AS TeamID,
    t.Name AS TeamName,
    t.City AS TeamCity,
    t.Coach AS TeamCoachID,
    c.Coach_ID AS CoachID,
    c.Name AS CoachName, -- Coach Name
    c.Experience AS CoachExperience,
    s.Stadium_ID AS StadiumID,
    s.Name AS StadiumName, -- Stadium Name
    s.City AS StadiumCity,
    ct.Contract_ID AS ContractID,
    ct.Start_Date AS ContractStartDate,
    ct.End_Date AS ContractEndDate,
    ct.Salary AS ContractSalary,
    tr.Trade_ID AS TradeID,
    tr.From_Team AS TradeFromTeamID,
    tr.To_Team AS TradeToTeamID,
    tr.Trade_Date AS TradeDate,
    se.Season_ID AS SeasonID,
    se.Year AS SeasonYear,
    g.Game_ID AS GameID,
    g.Game_Date AS GameDate,
    g.Home_Team AS HomeTeamID,
    g.Away_Team AS AwayTeamID,
    g.Stadium_ID AS GameStadiumID,
    g.Season_ID AS GameSeasonID,
    a.Award_ID AS AwardID,
    a.Type AS AwardType,
    td.Division_ID AS TeamDivisionID,
    d.Name AS DivisionName,
    st.Stat_ID AS StatID,
    st.Passing_Yards AS PassingYards,
    st.Rushing_Yards AS RushingYards,
    st.Receiving_Yards AS ReceivingYards,
    st.Passing_Touchdowns AS PassingTouchdowns, -- Passing Touchdowns
    st.Rushing_Touchdowns AS RushingTouchdowns, -- Rushing Touchdowns
    st.Receiving_Touchdowns AS ReceivingTouchdowns, -- Receiving Touchdowns
    st.Interceptions AS Interceptions, -- Interceptions
    st.Fumbles AS Fumbles, -- Fumbles
    i.Injury_ID AS InjuryID,
    i.Description AS InjuryDescription,
    i.Recovery_Time AS RecoveryTime,
    trr.Record_ID AS RecordID,
    trr.Wins AS Wins, -- Wins
    trr.Losses AS Losses, -- Losses
    trr.Ties AS Ties
FROM 
    Player p
    LEFT JOIN Team t ON p.Team_ID = t.Team_ID
    LEFT JOIN Coach c ON t.Coach = c.Coach_ID
    LEFT JOIN Stadium s ON t.City = s.City
    LEFT JOIN Contract ct ON p.Player_ID = ct.Player_ID AND t.Team_ID = ct.Team_ID
    LEFT JOIN Trade tr ON p.Player_ID = tr.Player_ID
    LEFT JOIN Game g ON g.Home_Team = t.Team_ID OR g.Away_Team = t.Team_ID  -- Join Game to Team directly
    LEFT JOIN Season se ON g.Season_ID = se.Season_ID  -- Game -> Season connection
    LEFT JOIN Award a ON a.Player_ID = p.Player_ID
    LEFT JOIN Team_Division td ON td.Team_ID = t.Team_ID
    LEFT JOIN Division d ON d.Division_ID = td.Division_ID
    LEFT JOIN Statistics st ON st.Player_ID = p.Player_ID
    LEFT JOIN Injury i ON i.Player_ID = p.Player_ID
    LEFT JOIN Team_Record trr ON trr.Team_ID = t.Team_ID AND trr.Season_ID = se.Season_ID
ORDER BY p.Player_ID, se.Year;

![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%201%20(2).png?raw=true)
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%201.png?raw=true)

Query #2: Show all the players that have a contract worth more than $200 million, that also have more than 8 wins, have not suffered an injury, and have won an MVP award
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%202.png?raw=true)

Query #3: Show all the players that have at least one receiving and one rushing touchdown that play in an NFC division with more than 5 wins
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%203.png?raw=true)

Query #4: Show all players traded in the month of October, along with their team record, height, weight, and touchdowns, that have not suffered not an injury
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%204.png?raw=true)

Query #5: Show all players in that have been injured that have at least 100 passing, rushing or receiving yards while showing their height, weight, and yards. Group each player by division and order them in descending order of yards
![Alt text](https://github.com/cacolquitt/MIST4610_Project2/blob/main/Query%205.png?raw=true)
