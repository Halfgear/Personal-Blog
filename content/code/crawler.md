---
title: "Building a Match History Crawler"
date: 2023-08-20
draft: false
github_link: "https://github.com/Halfgear"
author: "HwiJoon Lee"
tags:
  - Valorant
  - Python
  - MongoDB 
image: /posts/League/valorant.jpeg
toc:
---
## Introduction

Before I check my insights of Valorant with stats, the foremost task was to construct a robust data crawler. This pivotal first step would act as the foundation, paving the way for any meaningful analysis. When building a data crawler to fetch, process, and store a massive amount of data, challenges are inevitable. As I embarked on my journey to create a crawler for Valorant's match data, I encountered a series of obstacles that pushed me to optimize my approach in terms of speed, storage, and robustness.

![image.png](/posts/Valorant/image.png)

## Maximize Crawler Speed

A critical limitation with Riot games data crawling is often the rate to retrieve data from their server. I had to maximize what I could get from the low API limit for demo product if I wanted to gather vast amounts of data. My initial solution was straightforward: send a request, get data, process, and repeat. However, this sequential approach wasn't cutting it.

### Concurrent API Requests with Aiohttp

By using **`aiohttp`**  , asynchronous HTTP client/server framework, I was able to send multiple requests concurrently. This approach was similar to having multiple data crawlers working simultaneously to call match info API. This dramatically increased the amount of data I could retrieve within my request limits. I was able to confirm it when I was receiving ‘429 error’, which means I was hitting maximum number of requests.

![429.png](/posts/Valorant/429.png)

## Oversized Documents

Upon fetching the data, I noticed an immediate storage issue. The raw data I initially saved to MongoDB consumed a staggering 200KB-300KB per document. The voluminous nature of Valorant's API output with nested DTOs detailing every individual round, wasn't storage-friendly. Persisting with this raw format threatened to exhaust the storage capacity of our AWS machine. This was especially concerning as my crawls encompassed hundreds of matches in each loop. I tried to gzip the code to compress the data, but the zip was not very efficient because it was not recognizing repetitive patterns at the complicated API output to reduce the size.

### Trimming and Extraction

I took a methodical approach, scouring the data to pinpoint areas ripe for trimming while ensuring the quality and integrity of our information remained untouched. Unused details were discarded, while critical stats underwent extraction and condensation. My efforts culminated in a drastically trimmed document size around 12KB.

![collection](/posts/Valorant/size.png)

### Code

Let’s delve into how I managed this feat in the code:
```python
def extract_player_stats(players, round_results):
   #Extract and clean player stats from provided data.
    damage_dict = {player["puuid"]: [] for player in players}
    economy_dict = {player["puuid"]: [] for player in players}

    for round_result in round_results:
        for player_stat in round_result["playerStats"]:
            stat_puuid = player_stat["puuid"]
            # List comprehension to simplify the damage_dtos.
            damage_dict[stat_puuid].append([
                {k: v for k, v in dto.items() if k != 'receiver'} 
                for dto in player_stat["damage"]])
            economy_dict[stat_puuid].append(player_stat["economy"])
      
    # Add the extracted data for the player
    for player in players:
        puuid = player["puuid"]
        player["totalDamageStat"] = condense_damage_list(damage_dict[puuid])
        player["pistolRoundEconomy"] = get_pistol_round_economy(economy_dict[puuid])
        del player["playerCard"]
        del player["playerTitle"]
        del player["partyId"]      
    return players
```
I crafted the **`extract_player_stats`** function with a clear focus—efficiently distill player statistics from the given data. Here's what went on inside:

A damage dictionary (**`damage_dict`**) and an economy dictionary (**`economy_dict`**) were initialized for each player.

Round by round, I saved the respective player's economic standing and damage records while removing unnecessary IDs.

Unnecessary player attributes such as **`playerCard`**, **`playerTitle`**, and **`partyId`** were removed, considering they don’t have any subsequent statistical usage.

```python
def condense_damage_list(rounds_damages) -> list:
    total_damage = 0
    leg_shots = 0
    body_shots = 0
    head_shots = 0
    for round_damages in rounds_damages:
        if (len(round_damages) != 0):
            for damage in round_damages:
                total_damage += damage["damage"]
                leg_shots += damage["legshots"]
                body_shots += damage["bodyshots"]
                head_shots += damage["headshots"]
    output = {
        "damage": total_damage,
        "headshots": head_shots,
        "bodyshots": body_shots,
        "legshots": leg_shots
    }
    return output
```
The **`condense_damage_list`** function extracted all damges in each round into one dense damage detail of all round damages for each player.
```python
def get_pistol_round_economy(player_economy: list) -> list:
    try:
        output = []
        first_pistol_round = player_economy[FIRST_PISTOL_ROUND]
        if(first_pistol_round["spent"] != 0):
            output.append(first_pistol_round)

        second_pistol_round = player_economy[SECOND_PISTOL_ROUND]
        if(second_pistol_round["spent"] != 0):
            output.append(second_pistol_round)

        return output
    except IndexError:
        return output
```
Initially, I was planning to analyze pistol round purchases to find, which pistol and skills are preferred in agents, which might help us to understand the agent’s play style and their signature skill. So, I used the **`get_pistol_round_economy`** function to specifically extract this and reduce the size of the saved data.

```python
def clean_round_result(round_results):
    for round_result in round_results:
        for key in [
            "plantPlayerLocations",
            "plantLocation",
            "defusePlayerLocations",
            "defuseLocation",
            "bombPlanter",
            "bombDefuser",
            "playerStats"]:
            del round_result[key]
    return round_results
```
The **`clean_round_result`** function took center stage here. It looped across all round results, discarding chunks of extraneous data, including **`plantPlayerLocations`**, **`plantLocation`**, and a slew of other keys.

Through the integration of these refined functions, particularly within the **`save_match_info_to_db`** process, I achieved a MongoDB document that was not only lightweight but also condensed analysis-ready information.

## Error Handling

Even with optimized data fetching and storage, a crawler still had issues. During my testing phases, I encountered various API fails due- rate limits, temporary server issues, and unexpected response formats.

![crawler](/posts/Valorant/crawler.png)

### Comprehensive error handling

Instead of allowing these errors to halt my crawler, I implemented mechanisms to detect and handle them accordingly. From waiting out a rate limit with **`asyncio.sleep()`** to retrying requests in the face of server errors, my crawler became resilient. Each error-handling iteration made the system more robust, ensuring continuous data retrieval despite temporary setbacks if I have encountered them before.

### Conclusion

![MongoDB](/posts/Valorant/MongoDB.png)

By harnessing the power of asynchronous programming, fine-tuning data storage methods, and implementing robust error-handling mechanisms, I was able to optimize my data crawler. After reading API documentation multiple times, I was able to identify and extract the precise data I needed. This journey reminded me about the importance of perseverance, adaptability, and innovation in the face of technological hurdles. As developers, it's these trials that hone my skills, pushing me not just to build, but to build better.

![MongoDB](/posts/Valorant/kafka.jpg)

This was just a first step of the project. Upon completing the website backend support using NestJS with mocking data for front end developers to use, I intend to further advance the crawler system. A planned integration with Kafka will compartmentalize the crawler and statistical calculation components, ensuring that our infrastructure remains robust even if potential failures occur in our AWS machines. Furthermore, with a scheduled cronjobs, I will synchronize our data with PostgreSQL to finialize the backend support for our website.