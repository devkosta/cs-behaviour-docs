## Literature Review Notes

**Skill, Matchmaking, and Ranking Systems Design (GDC, 2017)**
https://www.youtube.com/watch?v=-pglxege-gU
- Skill depth is implemented into games to add replayability, ensuring that the player always has something new to learn.
- Definitions, as defined by Activision's Josh Menke:
	- Skill System:
		- Figuring out how good players are.
	- Matchmaking System:
		- Putting players together into matches.
		- It might use a skill system or ranking system.
		- Influences skill and ranking system.
	- Ranking System:
		- Telling player's how "good" they are.
		- It might use a skill system.
- What is ELO?
	- Created by Arpad Elo, the ELO rating system is a method of calculating the relative skill levels of players in zero-sum games such as chess.
	- Elo’s system constituted an improvement on the previous Harkness System.
	- Elo’s system was adopted by the FIDE (World Chess Foundation) in 1970.
- Ranking:
	- We know how good players are, what should we tell them?
		- Depends on the game.
		- Progression, Hybrid, or Skill.
	- Ranking Systems - Skill:
		- Ranks tied to skill.
		- Focus on current ability rather than progression.
		- Clearly competitive games (eSports focused games).

**Ranking Systems: Elo, TrueSkill and Your Own (GDC, 2019)**
https://www.youtube.com/watch?v=VnOVLBbYlU0
- A simple ELO rating system would be implemented as follows:
	- After a given match, rating points are transferred between players:
		- *RatingDiff = (Score - Expected) \* K-factor*
	- Where:
		- **Score** is 0 = loss, 0.5 = draw, 1 = win.
		- **Expected** is 0 to 1, the probability of winning.
		- **K-factor** is a constant for maximum change (update “speed”).
	- Much of the trick is in figuring out what the Expected result of a game is. The original ELO system uses the following formula (from the Normal dist.):
	- Between two players A and B, the expected probability of winning is calculated as follows:
		- *Expected[A] = 1/(1+10^(Rating[B-A]/400))*

**Matchmaking for Engagement: Lessons from Halo 5 (GDC, 2019)**
https://www.youtube.com/watch?v=0FoG4Jtpebs
- What is engagement?
	- Engagement means to be greatly interested.
	- Primarily accomplished by gameplay.
	- Matchmaking guarantees gameplay experienced as intended.
	- Avoiding disengagement is vital for the success of a game.
- **Note: This segment on engagement brings up a good point related to skill-based matchmaking. If there is an imbalance in ranks between two parties, it is likely that this will impact the engagement of players as they’ll be discouraged to continue playing.**
- Maintaining a low skill gap between players in skill-based matchmaking is important as it encourages players to continue playing.

**Quantifying CSGO Game Sense Using CSKnow Data: A First Step (David Durst, 2021)**
https://davidbdurst.com/blog/csknow_gamesense.html
- David Durst created CSKnow, a spatio-temporal database to create Counter Strike: Global Offensive (CS:GO) analytics, to quantitively measure game sense.
- There are many different skills that a CS:GO player must combine to be successful. These include:
	- Reaction Time
	- Game Sense
	- Teamwork
- This article focusses on breaking down one particular skill: **Game Sense**.
- Intuitive Definition of Game Sense:
	- Since CS:GO is a game of [incomplete information](https://en.wikipedia.org/wiki/Complete_information), the qualitative definition is based on how confident a player is in different parts of their game sense.
	- Durst breaks down the intuitive definition of game sense by assigning a confidence level to an individuals awareness of their surroundings.
	- For example, any competent player should be **~100%** confident/aware of their teammates position, health, and weapons.
	- Also, any competent player should be **~100%** confident/aware of the enemies metadata as the in-game HUD (heads-up display) displays the number of enemies alive and the positions of enemies considered "visible on radar".
- Quantitative Definition of Game Sense
	- From this intuition, Durst is able to derive a quantitative definition by combining the properties/metrics mentioned.
	- *a1 \* teammates_metric + a2 \* enemies_metadata_metric + a3 \* enemies_heard_metric*

**Challenges In Computing Reaction Time From CSGO Demo Files (David Durst, 2021)**
https://davidbdurst.com/blog/csknow_demo.html
- Reaction time is an important metric for understanding player behavior.
	- Better players should have better reaction time.
	- Unreasonable reaction times indications may indicate cheating.
- Durst defines reaction time as the "number of ticks from when an enemy is visible until a player's crosshair is on an enemy".
- **Note: A more accurate way to measure reaction time would be the "number of ticks from when an enemy is visible until a player's crosshair is within some radius of the enemy".**
- Challenges in computing visibility from CS:GO demo text data:
	- Demos store a text field called *spottedBy*, which is supposed to indicate when a player becomes visible to another player.
	- However, it is incredibly inaccurate and insufficient for computing visibility in reaction time.
		- They rely on approximate definitions of visibility that are updated less frequently and less precisely than the character models that are drawn on screen when replaying a demo.
	- Hence, Durst seeks another method.
- Computing visibility from a CS:GO demo's video data:
	- An alternative approach to computing visibility is to replay the demo file and record when enemies were visible in the video.
	- Durst utilises a combination of the HLAE mod, an extension of the CS:GO GOTV demo which allows for further functionality, and a computer vision (CV) algorithm.
	- By adding a custom shader to the demo file, assigning different colours to each of the enemies character models, and running the CV algorithm over the video, Durst was able to achieve a slightly more accurate measurement of visibility. 
	- However, it was still too inaccurate to measure 150-300ms reaction times.

**Quantifying CSGO Game Sense Using CSKnow Data: Evaluating A Player (David Durst, 2021)**
https://davidbdurst.com/blog/csknow_real_player.html
- This article explains how to evaluate a players gameplay using the model described in Durst's first article.
- Durst proposes a method to evaluate players by considering both players' perception and the actions they take as a result of those perceptions (described in the pipeline below).
	- *https://imgur.com/a/pY4taBs*
	- The first step is game sense, where players perceive the game by seeing images rendered to their monitor and sounds played in their headphones.
	- Then they make decisions based on their game sense.
	- Finally, the players take action based on those decisions. Their actions are their in-game behaviors.
- Defining behaviors:
	- Durst claims that player's behavior in CS:GO can be divided into four actions: holding, peeking, rotating, and utility.
- Evaluating decisions by behaviors:
	- Durst uses the behavior to evaluate decisions by treating each behavior as a guessing game.
		- Correct decisions imply good game sense for perceived information.
		- Incorrect responses imply bad game sense for perceived information.
		- The games are: holding, peeking, rotating, and utility.

**Computing Good Crosshair Placements Using RLpbr’s GPU Ray Tracing (David Durst, 2021)**
https://davidbdurst.com/blog/csknow_cover_edge.html
- "Good" crosshair placement is a crucial skill of skilled CS:GO players as it enables faster reactions. It does so by decreasing the amount of crosshair movement required when an enemy appears.
- Expert players are constantly adjusting their crosshair placement, even when no enemies are visible.
- The predictive crosshair placement process focuses a player's crosshair at a likely location where an enemy will appear.
- Predictive crosshair placement is challenging because it depends on a lot of context:
	- A players location
	- The maps geometry
	- Enemies locations
	- Teammates crosshair placements
- In this article, Durst proposes/defines an algorithm for computing the predictive crosshair placement corners where enemies may appear based on the players position and the maps geometry.
- Algorithm Inputs:
	- Let *G* be the map's geometry, such as floors and walls.
	- Let *P* be all the places a player can stand.
	- Let *V* be the voxels that discretize all possible locations for a character model in the map, even in the sky or under the ground.
	- Let *Q* be the voxels that discretize all the places a player can stand in the map.
	- Let *E* be all possible locations for a player's eyes.
- Algorithm Outputs:
	- With these definitions, Durst formalizes the concept of "locations where enemies may appear".

**Reaction Times And Crosshair Positions Aren't Enough To Catch Cheaters (David Durst, 2021)**
https://davidbdurst.com/blog/csknow_analytics_anticheat.html
- Using analytics to find cheaters:
	- Utilising behavioral patterns such as reaction time and crosshair placement are interesting as they are generally the two main reasons why players are accused of cheating.
	- A too perfect reaction occurs when a player aims at an enemy before they become visible.
	- A too perfect crosshair position occurs when:
		1. A player has many crosshair position options since they are exposed to enemies possibly appearing from any angles.
		2. The player chooses the correct crosshair position for the one angle where an enemy will appear.
- Durst proposes an anti-cheat model based on this intuition that too perfect reaction times and crosshair positioning indicate cheating.
- The goal of the model is to group players into three cohorts:
	- **Pros:** Near-instantaneous reaction times due to few crosshair position options. Pros limit the number of crosshair positions by standing in locations where enemies can only appear from a few angles.
	- **Hacking Amateurs:** Negative reaction times even though there are many crosshair positions may indicate cheating.
	- **Legit Amateurs:** Slow, positive reaction times due to many cross position options may indicate amateur behavior.
- Results of Durst's testing:
	- The pros, hacking amateurs, and legit amateurs reaction times matched the expected results.
	- The crosshair position results were however less successful, not producing "blog-worthy" charts.
		- It was found that all players had roughly the same average.