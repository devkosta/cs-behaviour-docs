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

**Bayes Esports: How to measure esports’ natural born killers (H.B. Duran, 2021)**
https://esportsinsider.com/2021/09/bayes-esports-how-to-measure-esports-natural-born-killers/
- Article written by Ben Steenhuisen, Senior Software Architect at Bayes Esports.
- Provides a high-level overview of measuring skill-level/in-game performance in the context of Counter Strike: Global Offensive (CS:GO).
- Giving KDR Context:
	- Due to the nature of CS:GO, the most commonly discussed player performance statistics are kills and deaths.
	- Instead of just accounting for KDR (kill-death-ratio), we need to see the exact context in which they occur, since every single kill and death happens in different situations.
	- A key example is the occurrence of ECO rounds, where one team can not afford to purchase expensive equipment. This team can attempt to be aggressive with just their (free) starting pistols - knowing the odds of success are slim but their investment into the round is minimal. If the team dies, should these "kamikaze" deaths be counted towards the player's performance?
	- Meaning that when people are discussing the performance of a player, the primary statistics considered are currently *Total Kills* and *Total Deaths*.
	- The way in which we look at these kills and deaths are primarily as aggregates - without any contextualization of the individual events that they represent. 
- The only worthwhile approach to this conundrum is to decompose a round into its discrete events (such as kills, deaths, bomb plants, bomb defuse attempts, saving a gun for a future round, movement around a map, etc.).
	- Using these events, we can evaluate the impact on key indicator metrics like Round Win Percent (short term), or Map Win Percentage (a longer-term metric), and then wrap each up separately as aggregates.
- Initial Research Attempts:
	- In the initial research on this topic, they decided to keep it simple and quantify each separate kill and death event using a slimmed-down round winner probability model, ignoring propagation between events.
	- Over 170K rounds of gameplay were parsed from 6423 CS:GO professional matches. For each kill and death events they evaluated the round win percent metrics before and after the death event.
		- *https://imgur.com/a/NYDmmwx*
		- Taking the average net increase in round win percent from kills and subtracting the shift due to deaths, you're left with a "round impact" for a player.
	- This model can be expanded upon by evaluating more event types, as mentioned above, and there can also be a differentiation between the overall goal of a game of CS:GO which is to win 16 rounds, versus the obvious implicit subgoal which is to win the current round.
	- Another interesting topic is quantifying these underlying statistics based on the strength of an opposing team - killing a player on a skilled team is more difficult than killing a player on a less skilled team.

**Level Difficulty and Player Skill Prediction in Human Computation Games**
https://www.aaai.org/ocs/index.php/AIIDE/AIIDE17/paper/viewFile/15851/15203
- Human Computation Games (HCGs) often suffer from low player retention.
- This may be due to the constraints placed on level and game design from the real-world application of the game.
- Previous work has suggested using player rating systems (such as Elo, Glicko-2, or TrueSkill) as a basis for matchmaking between HCG levels and players, as a means to improve difficulty balancing and thus player retention.
- In this paper, features derived from player behavior and level properties were examined to predict their Glicko-2 ratings in the HCG *"Paradox"*.
- Rating systems typically start incoming entities (i.e. players or levels) with a default rating.
	- They are essentially treated all the same - a "blank state" about which there is not information.
	- However, in the case of HCGs, there may be some information about incoming entities that can be used to inform their initial rating.
		- For players, this can include their behavior in the game up to the point where a rating is needed - for example, their performance in any tutorials or onboarding before they are to be matched with a level derived from a real task.
- For use in a rating system, they treated each instance of a player successfully completing a level as a win for the player, and a player failing to complete a level as win for the level.
	- Thus the most difficult levels are those that "win" the most, and therefore end up with the highest ratings, similar to a player-vs-player (PVP) setting.
	- Hence, using rating systems offers a reasonable way of assessing both level difficulty and player skill, acting as a suitable ground truth on which to make predictions.
- A Linear and Gaussian progress regression was used to predict the ratings of players - based on their behavior in the game's tutorial levels - and levels - based on properties of the underlying problem defining the level.
	- It was found that rating predictions based on regression were closer to the actual ratings than both the default Glicko-2 rating and a base-line method using average ratings.
	- Predictions generally performed better for level ratings than for player ratings.
- Dynamic Difficulty:
	- Although the immediate goal of the work in this paper is predicting player and level ratings, this will be applied to matchmaking between players and levels, with the goal of assigning levels of appropriate difficulty to players based on their skill.
	- This relates to dynamic difficulty adjustment, which has received considerable attention in the game research community.
	- Inspired by the flow theory of Csikszentmihalyi (1990), dynamic difficulty adjustment can attempt to affect player engagement by personalizing the difficulty of a game for each player’s skill. Much work has examined the relationship between skill, difficulty, and engagement (Engeser and Rheinberg 2008; Alexander, Sear, and Oikonomou 2013; Denisova and Cairns 2015; Lomas et al. 2013).

**Using Machine Learning to Predict Game Outcomes Based on Player-Champion Experience in League of Legends**
https://arxiv.org/pdf/2108.02799.pdf
- An important aspect of League of Legends (LoL) is competitive ranked play, which utilizes a skill-based matchmaking system to form fair teams.
- Fair matchmaking is crucial for player experience.
	- Riot Games stating that some of their main goals for the year of 2020 were to preserve competitive integrity and improve matchmaking quality.
- LoL matchmaking is determined using an Elo rating system, similar to the one originally used by chess players.
	- Although this matchmaking system has improved in recent years, it does not consider players' champion selections when forming matches.
	- LoL has 150 playable characters, known as champions, that have their own unique playstyles and abilities.
	- Some player will often perform better on some champions than on others due to their differing levels of mechanical expertise, which is defined as a player's knowledge of their champion's abilities and interactions.
		- Higher levels of mechanical expertise on particular champions allow players to make quicker and better judgements, which are essential in the game's fast paced environment.
	- Since mechanical expertise plays such a large impact on a player's own performance, it can therefore cause a similar impact on the match's outcome.
- This paper introduces a machine learning model based on a deep neural network (DNN) that can predict ranked match outcomes based on player's experience on their selected champion (i.e. player-champion experience).
	- They show that the outcome of a ranked match can be predicted with over 75% accuracy after all player's have selected their champions.
	- The results indicate that individual champion skill plays as significant role in the outcome of a match, regardless of team composition.
- In addition, current matchmaking may not form teams of approximately equal skill level in game because matchmaking is done before players select their champions.
	- The papers results imply that players should only play champions that they have mastered in order to win ranked games.
	- This can decrease gameplay variety as player typically master only a few champions out of the 150+ available.

**Predicting behavioral competencies automatically from facial expressions in real-time video-recorded interviews**
https://www.researchgate.net/publication/348819375_Predicting_behavioral_competencies_automatically_from_facial_expressions_in_real-time_video-recorded_interviews
- This work aims to develop a real-time image and video processor enabled with an artificial intelligence (AI) agent that can predict a job candidate's behavioral competencies according to his or her facial expressions.
- This is accomplished using a real-time video-recorded interview with a histogram of oriented gradients and support vector machine (HOG-SVM) plus convolutional neural network (CNN) recognition
- Competency refers to "a set of behavior patterns that the incumbent needs to bring a position to perform its tasks and functions".
	- This can help to predict how a job candidate will perform or behave at a specific job for which he or she is applying.
	- Therefore, competency is also called behavioral competencies that reflect the attributes underpinning a behavior, including knowledge, skills, abilities, and other characteristics associated with successful performance in an area of work.
- There are different approaches for developing competency models in human resource management.
	- The job-based approach.
	- The future-based approach.
	- The person-based approach.
	- The value-based approach.
- The results indicated that their proposed system can provide better predictive power than can human-structure interviews, personality inventories, occupation interest testing, and assessment centers.
- **Note: I think that this analysis of "job duties and requirements" in the "job-based approach" may work well in the context of team-based competitive games where each player is assigned a role (e.g. League of Legends has a specified role for each player - top, bottom, mid, jungle, and support).**