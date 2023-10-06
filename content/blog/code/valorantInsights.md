---
title: "Enhanced Valorant Metrics"
date: 2023-10-05
draft: true
author: "HwiJoon Lee"
tags:
  - Valorant
image: /posts/League/valorant.jpeg
description: "New metrics developed by researching valorant API"
---
## Agent Based Rating
- 평균전투점수 공식
 - 피해량 당 1점
 - 생존한 적숫자에 따른킬 150/130/110/90/70
 - 다중킬 추가킬당 50점
 - 피해를입히지 않은 어시스트 25점

- 이 전투점수 시스템은 전반적인 전투 기여도는 잘 보여주나 요원과 포지션별 특징을 반영하지 못함
- 레이즈, 제트같이 먼저 진입하여 초반 전투에 관여하거나 레이나 같이 모든 스킬이 킬을 많이 따는데 집중되어 있을시 평균 점투점수가 높을 수 밖에 없는 구조
- (이미지 첨부 예정)
- 그래서 유저의 승리 기여도를 평균전투점수로 등수를 메겨서는 안되고, 요원별로 기대되는 평균전투점수를 기반으로 순위를 매겨야 한다.

고려해야할 사항
- 각 요원별 전투점수를 가우시안 분포로 정리해두어 유저의 점수가 상위 몇프로인지 확인하여 Rating을 부여할 예정
- 피해량도 계산에 들어가, 방어구를 많이사는 후반라운드를 많이 했을수록 10명 모두의 전투점수가 전판적으로 올라간다.
    - 이를 고려하여 라운드 몇판했는지에 따른 normalizaion도 해야한다

## Trade
- 트레이드는 기본적으로 한명의 팀원이 적에게 처치당했을때, 본인의 팀에서 그 적을 빠르게 처치하여 상황을 균형있게 만드는 것을 의미합니다.
- 좋은 트레이드는 숫자적으로나 전략적으로 불리한 상황을 다소 완화 시킬 수 있습니다.
- 트레이드는 팀워크, 팀원 위치 인지능력, 빠른 반응성을 필요로 하며 게임에서 중요한 역할을 하며 높은 레벨의 경기일수록 더욱 중요해집니다.
- 아군이 죽었을때 트레이드 해주는 능력과 아군이 트레이드 해줄 수 있는 환경 만들어 죽어주는것 또한 발로란트 실력중 중요한 지표가 될 수 있습니다.
- 현재 모든 발로란트 웹사이트를 확인해 보았을때 아직 트레이드에 관한 지표를 확인하는 곳은 보지 못했습니다.
- (파이썬 코드 첨부 예정) 이런 방식으로 트레이드를 통해 얻은 킬과 데스를 각 플레이어 별로 기록 할 수 있다는걸 알았고, 이를 지표화하여 보여줄 예정입니다.

## Composure
- 발로란트는 기본적으로 전략적 순환이라는 과정을 거치는 유저들을 incentify한다. 일단 정보를 모으고, 다음 행동을 계획하고 실행하는 플레이를 장려하는 시스템을 가지고 있다고 발로란트 개발자들은 항상 강조를 한다.
- 저 순환을 이해하고, 침착하게 수행하는 플레이어에 대한 지표를 만들고 싶었다.
- 발로란트에서 어려운 클러치를 해낸다는건 내가 생각하기에는 굉장히 어려운 하나의 퍼즐을 푸는 것과 같다고 생각한다.
- 그 어려운 클러치를 해내는 능력을 쉽게말해 침착성이라고 말할 수 있다고 생각한다.
- 그런 점에서 나는 침착성을 구하기 위해, 모든 매치를 크롤링 할때마다, 각 유저의 클러치 횟수를 얻어서 기록해두었고, 몇명 상대의 클러치인지도 기록해둬서, 더 많은 인원 상대의 클러치일시, 점수를 더 부여하는 식으로 침착성이라는 metrics를 구해줄 예정이다.
- 클러치횟수는 이런 방식을 통해서 구했다. (코드 및 설명 첨부 예정.)

![tacticalCycle](/posts/Valorant/tacticalCycle.jpeg)


## Agent-Based Rating: A Closer Look at its Nuances
Traditional combat score systems, often calculated by metrics like damage per point, kill points depending on the number of enemies left, multi-kills, and assists, do provide an overview of a player's combat effectiveness. However, they fall short of capturing the unique abilities and roles of different agents like Raze, Jett, and Reyna. These agents often excel in specific scenarios which can disproportionately inflate their average combat score. Therefore, one can argue that simply ranking players based on average combat scores would not be wholly fair or reflective of their contributions to victories.

One way to address this could be to standardize each agent's combat scores using Gaussian distribution. This allows us to see where a user's score stands in comparison to others who play the same agent. Another element to consider is round-specific normalization, as armor purchases in later rounds can inflate all players' combat scores.

## The Underrated Skill: Trade
Trade, the act of quickly eliminating an enemy who has killed a teammate, is a skill often overlooked but is crucial in high-level games. A successful trade can offset numerical disadvantages, enabling a more balanced combat scenario. This aspect requires keen teamwork, spatial awareness, and rapid response time. Though most Valorant stat websites don't provide metrics for trading, it’s an essential skill that can be quantified. By capturing the number of trades made by each player through Python coding (code to be attached), a detailed analysis of this skill can be offered.

## Composure: The Make or Break Metric
Valorant is structured in such a way that it incentivizes strategic cycles; players are encouraged to gather information, plan, and then execute their strategies. Developers emphasize this as the game's core loop. Understanding and staying calm throughout this loop is what we can term as 'composure.' In Valorant, pulling off a difficult clutch is akin to solving a complex puzzle under time pressure.

To quantify composure, one can track the number of clutches a player wins and the number of enemies they were up against during the clutch. A more significant number of enemies should naturally weigh more heavily in the score. This metric can be achieved through careful data scraping of each match, specifically capturing each player's clutch moments.

