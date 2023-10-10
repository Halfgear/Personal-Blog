---
title: "Enhanced Valorant Metrics"
date: 2023-10-05
draft: false
author: "HwiJoon Lee"
tags:
  - Valorant
image: /posts/Valorant/cypher.jpeg
description: "New metrics developed by researching valorant API"
---
## Agent-Based Rating

In combat score systems, metrics such as damage per point, kill points based on remaining enemies, multi-kills, and assists offer a general overview of players’ combat effectiveness. However, this approach overlooks the nuanced roles and abilities of different agents like Raze, Jett, and Reyna, who may excel in specific conditions that disproportionately inflate their combat scores. Ranking players solely on these scores would neither be equitable nor accurately represent their contributions to team victories.

![avs](/posts/Valorant/avs.png)

### Data Interpretation
The accompanying Excel chart displays average combat scores for each agent, collated from real-time game data via my [crawler](https://jooncode.com/blog/code/crawler/). Notably, duelists such as Raze, Jett, and Reyna generally scores higher average combat. This is not an indication of superior skill but rather an inherent bias in scoring metrics toward agents designed for aggressive, high-damage roles. Conversely, Yoru and Neon have a higher skill threshold and their capabilities demand precise execution, which lead to overall lower average combat scores.

![avs](/posts/Valorant/normalDistribution.jpg)

### Statistical Approaches

To offer a more equitable assessment of player contributions, two advanced statistical methods are required. Firstly, Gaussian distribution can serve as a computational tool to level the playing field among agents. By calculating the mean and standard deviation for each agent’s scores, one can convert these into Z-scores. This standardization allows players to be ranked in context with those who play the same agent, neutralizing any inherent advantages or disadvantages specific to that agent.

Addtionally, Round-Specific Normalization introduces a dynamic scaling factor adjusted for game variables, such as equipment quality and round progression. This tackles the issue of score inflation at later rounds.


## Trading

While the typical stat lines in Valorant focus on simpler metrics like kills, deaths, and assists, trading represents a more nuanced skillset. Trading happens when you eliminate the enemy who has just killed your teammate, thereby balancing the numbers and potentially the outcome of a round. This skill gets more crucial in high-elo games where demand acute teamwork, situational awareness, and reflexes.

![trade](/posts/Valorant/trade_example.png)

### Limitation of Existing Metrics

Interestingly, some stat-tracking websites do record "trade kills," but they fall short by neglecting to capture "traded deaths." The distinction is crucial: dying in a position that allows a teammate to secure a trade kill adds strategic value that current metrics overlook. In contrast, a "untraded death," one that cannot be traded, lacks this additional layer of utility.

By using Python to scrape and analyze data, it becomes possible to quantify trading as a skill. One could capture not just the instances of trade kills but also record instances of traded deaths, thereby providing a fuller picture of a player's tactical value. This approach aligns with the earlier discussion on implementing Gaussian distribution and round-specific normalization to achieve a more comprehensive and equitable evaluation of player performance.

## Composure

In Valorant, the game is structured to incentivize a strategic loop: players are encouraged to gather information, plan, and execute their strategies. Developers consider this cycle to be the core gameplay loop. Maintaining composure is critical throughout this loop, especially in high-stakes moments. When faced a difficult clutch situation, those who keep their composure are more likely to navigate the challenges successfully, almost as if solving a complex puzzle under extreme time pressure.

![composure](/posts/Valorant/composure_image.jpg)

### Measuring Composure
Specifically, I propose tracking the number of clutches a player successfully wins, weighted by the number of enemies they were up against. To facilitate this, I can develop a score chart for 1 vs N clutches, with higher scores assigned to clutches against more enemies. This data would be collected through meticulous scraping of match records, focusing on each player's clutch moments. Each tier on the score chart will represent an expected value of composure along with its distribution. By identifying where a player falls within this distribution, we can offer an empirical measure of their composure in high-stakes situations.

![combat](/posts/Valorant/combat.jpeg)

## Conclusion
Traditional metrics are a good starting point but fall short in capturing the nuances of player performance in games like Valorant. By integrating these advanced statistical methods we can move closer to a truly reflective stat tracking system. The aim is not just more numbers, but better insights. This is an ongoing effort, but one that promises a more equitable and accurate representation of player skills and contributions.