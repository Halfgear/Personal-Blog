---
title: "From Gamer to Dev: Bridging the Gap for Valorant Newbies"
date: 2023-08-15
draft: false
github_link: "https://github.com/Halfgear"
author: "HwiJoon Lee"
tags:
  - Backend Development
  - Website Development
  - Valorant
image: /posts/League/meAtRiot.png
description: "From Valorant insights to my Riot internship aspirations: A data-driven journey"
toc:
---
## My Experience with Riot Games

For the past 10 years, Riot Games has been the cornerstone of my gaming experience. I've always cherished those competitive moments, playing alongside friends to clutch those hard-earned victories in League of Legends and Valorant or watching last year DRX’s amazing world winning story. My enthusiasm was so contagious that I naturally wanted more friends to join the fray. Yet, the highly competitive nature of these games made it challenging for me as an advanced player to guide them. I often found it tough to step into their shoes, to see what they were seeing and understand what they didn't yet know.

## Barrier to learn
![me](/posts/League/barrier.png)

For those new to the realm of Riot game, stepping into a universe that has evolved over 13 years can be incredibly overwhelming. With League boasting over 200 items, 164 champions, and each champion possessing at least five distinct abilities, the volume of the raw information to grasp is staggering. Presenting a beginner with these numbers is a easy way to trigger a sense of awe, and perhaps, intimidation.

### Don’t want to make it easy

One of the intrinsic appeals of gaming is the sensation of overcoming the stress and challenges presented to the player. The greater the challege that players overcome is the more profound the sense of satisfaction. Both League of Legends and Valorant offer intense levels of competition, leading sometimes to conflicts between randomly matched teammates in their first normal queue against people. Many players feel overwhelmed during their very first match and choose to avoid playing League rest of their life after the horrible experience. 

### What Should We Do Then?

Last year, Mahjong, though considered unfamiliar board game in Korea, surged in popularity on Korean Twitch streams because of a mobile game. Around then, a small website began to gain traction in twitch chat with the tagline, "Watch this, and you're set to play Mahjong with anyone!" Veteran players found it invaluable, frequently sharing it with newcomers before they start playing together. This sparked an idea in me: What if there existed a similar platform for games like League or Valorant? Such a tool could be the perfect way to convince my friends into my favorite games! Imagine the convenience for beginners, enabling the player group to quick intergration into matches alongside their more experienced friends.

<img src="/posts/League/matchHistory.png" style="display: block; margin-left: auto; margin-right: auto; width: 97%; height: 97%;"/>

Building on that thought, I’m developing a Valorant website as backend developer and product manager for my internship. Yes, there are numerous Valorant sites like tracker.gg that simply display past match histories and compute basic stats, such as headshot rates like the image above. But I wanted to show insights beyond that.


I want our website to be a starting point for beginners – a place where they can dive into curated content, gain the confidence to play alongside their friends, and understand their baisc in-game roles. Furthermore, I want to offer more than just the basics; I aim to provide insights into the game's environment and share intriguing facts. I'll be sharing into some of these captivating stats in this post.

## Beyond a Basic Tutorial

While tutorials offer a grasp on the very foundational controls, mastery over intricate strategies and positional understanding is vital for actual gameplay. Delving into specific insights can significantly enhance a beginner's experience when queued with veteran players. Furthermore, some information may help the veteran players to play with newbies in their games too.

Taking Valorant as an example, beginners often find themselves grappling with unfamiliar elements in games. The agent unfamiliarity tends to push them towards champions that appear more appealing and easy. A case in point is Sage. She heals and revives an ally with a point click, which seems very easy like Mercy in Overwatch. However, ironically, Sage is hard because of her strong supporting ability because it requires her to stay alive and farm ult points. I confirmed my thoughts when I analyzed the data from Valorant API with Python and MongoDB. 

<img src="/posts/League/sage.png" style="display: block; margin-left: auto; margin-right: auto; width: 97%; height: 97%;"/>

This chart is represents the agents’ win rate difference between high and low tier. Higher the bar means the agent performs better at high tier than low tier. The stark performance gap from Sage suggests that her perceived simplicity entices many newcomers, but it may not be the best agent for beginners.

### Is Presence of Smoke Crucial?

Meanwhile, veteran players, understanding the nuances and the importance of smokes, often prioritize incorporating it into their team strategies. On the other hand, newcomers, not entirely grasping this concept, tend to opt for more straightforward agents. These veteran players, aware of the challenges of attacking or defending without the right smoke placement, can sometimes express their frustration towards the newer players' agent selection or lack of understanding how to smoke.

I want to emphasize my significant observation to the veteran players: the presence or absence of smoke doesn’t always correlate with win rates! So, there's no need to agonize over crafting the 'perfect' team composition for your ranked games with random teammates.

<img src="/posts/League/smoke.png" style="display: block; margin-left: auto; margin-right: auto; width: 97%; height: 97%;"/>

This chart reveals that around 8% of rounds lacked a smoke agent (Controller) within a team composition. However, for games played at the platinum tier or above, this number shrunk to 3.24%. This suggests that higher-tier players prioritize having at least one controller in their lineup. If we could widely share these insights, it might alleviate the undue pressure players feel about team compositions. In turn, this could greatly enhance the gaming experience, especially when paired with random teammates.

### Wrapping Up the Insights

Throughout my journey from a beginner Sage player to an intermediate at the Platinum rank in Valorant, many insights gradually formed in my mind. While I've shared a few of these findings here, trust me, the rabbit hole goes much deeper. I've rigorously investigated almost every piece of data output from Valorant's match API to answer my curiosity about the Valorant statistics.

However, due to my contractual obligations with the company, there's a plethora of information I need to withhold – at least for now. That said, these insights will be instrumental in shaping the website we're currently developing, ensuring it becomes a rich resource for both newbies and veterans. I am very excited to bring these insights into the website!


## Conclusion
![me](/posts/League/meAtT1.png)

As a gamer and e-sports fan, I've experienced firsthand the thrill, strategy, and camaraderie games can bring. I genuinely hope more people come to appreciate and enjoy what games can offer.

In line with this passion, I have been harnessing my backend development skills to support the website development of my current internship and create a platform to bridge the huge gap between newbies and seasoned players. As next step, I will apply for a Riot 2024 summer internship to immerse myself further into a community and company I want to join to learn! But regardless of the outcome, my commitment will remain unchanged. Even if I don't secure a spot at Riot, my ambition is to persist in the gaming industry, especially in backend development roles, drawing from the experiences of my current internship.
