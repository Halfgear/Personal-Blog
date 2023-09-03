---
title: "Agent difficulty Paradox"
date: 2023-07-16
draft: false
github_link: "https://github.com/Halfgear"
author: "HwiJoon Lee"
tags:
  - Valorant
  - Python
  - MongoDB
image: /posts/League/valorant.jpeg
description: "Data investigation to find easy to play agents"
toc:
---

The realm of Valorant offers players numerous agents, each boasting unique abilities and strategies to win. But how do players, both new and experienced, navigate this vast selection? 

With a combination of Python programming and a rich dataset extracted from MongoDB, I obtained the performance metrics of various agents across different skill tiers. The result challenges widely held beliefs and shed light on the actual efficacy of agents in various gameplay scenarios. This post will offer insights that can inform and enhance player choices and strategies.

## Methodology
```python
match_db = riot_db.get_collection('competitive_match_crawler')
high_dict = {}
low_dict = {}
def create_dict():
    agent_id_list = char_dictionary.keys()
    for agent_id in agent_id_list:
        high_dict[char_dictionary.get(agent_id)] = {"win": 0, "loss": 0}
        low_dict[char_dictionary.get(agent_id)] = {"win": 0, "loss": 0}
    return
def loop_docs_count_win_loss():
    for document in match_db.find():
        count_win_loss(document)
    print("finished looping")
    return
```
``` python
def count_win_loss_in_document(document):
    players = document["players"]
    teams = document["teams"]
    
    blue_char_ids = []
    red_char_ids = []
    
    for player in players:
				char_id = player["characterId"]
				tier = player["competitiveTier"]
        if (player["teamId"] == "Blue"):
            blue_char_ids.append(char_id)
        else:
            red_char_ids.append(char_id)

    if (did_red_win):
        increment_win(red_char_ids, tier)
        increment_loss(blue_char_ids, tier)
    else:
        increment_win(blue_char_ids, tier)
        increment_loss(red_char_ids, tier)
    return

def increment_win(char_id_list, tier):
    for char_id in char_id_list:
        agent_name = char_dictionary.get(char_id)
        if(tier < 15):
            low_dict[agent_name]["win"] += 1
        else:
            high_dict[agent_name]["win"] += 1
```
### 1. Database and dictionaries Initialization
The data is fetched from my MongoDB collection which I am currently crawling with AWS EC2 machine with Valorant API. high_dict and low_dict are initialized as dictionaries to store win-loss statistics for agents across different skill tiers.

The function create_dict() systematically populates the high and low skill tier dictionaries with agent names, initializing their win-loss count with zeros. This created a base template for subsequent data aggregation.

### 2. Data Iteration
The loop_docs_count_win_loss() method ensures every match in the database is scanned, while the win-loss data for agents is populated through the count_win_loss_in_document() function.

### 3. Match Analysis:
Within each match document, agent details and team information are extracted. Depending on which team wins, the increment_win or increment_loss functions update the respective win or loss count for the agents.

The code accounts for the player’s competitive tier (below or above Platinum) to decide if the win or loss should be registered in the low or high skill tier dictionary.

<img src="/posts/League/sage.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%; height: 70%;"/>

The chart is drawn with google spread sheet and pandas library.

## Sage is diffcult

The chart above showcases the win rate difference between high and low-tier players for various agents. The taller the bar, the better the agent's winrate in higher tiers. The two data points that stand out most prominently in the chart are Sage and Neon. If you were to ask current Valorant players which agent they would recommend to a newcomer between these two, most would likely suggest Sage. 

<img src="/posts/League/sage_diff.png" style="display: block; margin-left: auto; margin-right: auto; width: 30%; height: 30%;"/>

Sage's abilities are straightforward, and even if one isn't skilled in direct combat, they can still contribute significantly through her supportive capabilities, which seems very easy like Mercy in Overwatch. 

However, the data tells a different story from this widely held belief. Sage's win rate eclipses her peers in the higher tiers by 1.744%, while Neon lags behind with a decrease of -1.406%. Notably, these two agents possess deviations that are double that of their closest competitor. Ironically, Sage is hard because it is difficult to maximize her supporting value because it requires her to stay alive and farm ult points with kills.

When players are unfamiliar with the available agents, they often gravitate towards those that appear simple and user-friendly. Sage, who seems to have a straightforward and intuitive skill set, is frequently recommended to newcomers. However, my recent statistical analysis suggests that this common recommendation may not be the most beneficial for new players.

## Flash difficulties
The chart predominantly clusters agents by their respective roles, with only a few edge cases like Neon. Notable difficulty disparity is the gap between Flash-holding Initiators and Non-flash Info Initiators.

<img src="/posts/League/initiator.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%; height: 70%;"/>

This notably underscores the intricacies of using the 'Flash' ability in Valorant. If flash is not deployed effectively during combat, it risks allies as well. The performance disparity of Flash-based agents across skill tiers reveals that proficient players employ the Flash with a tactical finesse that significantly contributes to team combat efficacy. Conversely, its misuse at low tiers can suggest unfamiliarity or even intimidation with the ability.

Conversely, the data also highlights that agents like Fade and Sova with drones, exhibit less performance gaps. This suggests that even when players at the lower end of the skill spectrum deploy these abilities, they can do so without severely impacting their team's outcome.

For newcomers, beginning with info agents like Sova could be a strategic move. This will allow them to familiarize themselves with the game's tempo and mechanics, setting a solid foundation for mastering more complex agents and abilities in the future.


## Conclusion

Navigating the intricate landscape of Valorant is not a small project. We identified what could possibly cause the performance gap between low and high tiers with data analysis supported Python and MongoDB. 

<img src="/posts/League/agents.jpeg" style="display: block; margin-left: auto; margin-right: auto; width: 70%; height: 70%;"/>

Traditional wisdom might suggest certain agent choices, but our data reveals nuances—like Sage, seemingly beginner-friendly, yet possessing a steep learning curve. It's a testament to the depths that lie beyond initial impressions.

Furthermore, understanding the challenges that confront newcomers is pivotal. We need to pave the way for enhancing their gameplay and easing their integration into the competetitive ecosystem. Through this analysis, I hope to bridge the knowledge gap for players, old and new, encouraging them to make data-informed decisions in their Valorant journeys. I have a lot more insights to share, so stay tuned!