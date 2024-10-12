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
