---
title: "Backend System Design"
date: 2023-08-29
draft: false
github_link: "https://github.com/Halfgear"
author: "HwiJoon Lee"
tags:
  - Backend
  - Postgresql
  - MongoDB
  - NestJS
image: /posts/Dev/backend.png
toc:
---
Building the backend infrastructure for our Valorant data-driven website, I encountered several unique challenges and oppurtunities to grow as a software developer. In this post, I'll outline our current implementations and the ongoing developments for our backend system design. 

## Pipeline with Kafka

In our quest to build an advanced and reliable infrastructure, Kafka became a cornerstone of our design. The integration effectively separates our crawler from our statistical calculation components, aiming for a system that remains robust, especially when it faces potential computational challenges.

![MongoDB](/posts/Dev/MongoStruct.png)

[***Our match crawler system***](https://jooncode.com/blog/code/crawler/) extracts Valorant game data, which undergoes a multiple parsing mechanism to reduce document size. This parsed data is stored in MongoDB. When a match is stored, a Kafka producer dispatches these match details to a designated topics.
    
### Data Processing
![MongoDB](/posts/Dev/dataProcessing.png)

Kafka consumers retrieve match id from the topic and initiate necessary calculations with the parsed data in our database. Some calculations might be straightforward and quick, while others could demand more computation.

### Balancing Workloads
![MongoDB](/posts/Dev/balance.jpeg)

Kafka's design allows for flexibility in managing computational demands. If a consumer faces a heavier workload, we can easily introduce additional consumers to that consumer group. This adaptive scaling ensures a more evenly distributed processing speed to optimize our system's efficiency. Additionally, we will be able to leverage consumer offsets to check the overall computing progresses. If there's a noticeable gaps in offsets between consumers, they indicate the potential processing bottlenecks to be addressed.

### Parallelism & Scalability
Kafka, by design, is the best for parallel data processing. This inherent trait ensures both scalability and concurrent processing for us. Even if faced with diverse data computation demands, our system remains agile and responsive.

## API with NestJs

![PSQL.jpg](/posts/Dev/PSQL.png)

Choosing the appropriate framework can greatly influence the trajectory of a project. NestJS offers us a defined set of rules and structure. It streamlines our processes by ensuring there's a consistent pattern we can use across different modules. This foundation becomes invaluable as projects grows, guaranteeing that the architecture remains consistent.

### Studying Entity Relationships

![agentWinrate](/posts/Valorant/agentWinrate.jpeg)

One of the core lessons revolved around constructing relationships for entities. For instance, my task for main page was to present statistics of every Agent at four different positions across all maps on our main page. This demanded a solid understanding about the complex business logic. While building this feature, I learned how to come up with precise entity Columns and relations to create an ideal output for frontend developer and validate APIs with swagger.

![swagger.png](/posts/Dev/swagger.png)

### More NestJS Features

NestJs required me to familiarize myself with features beyond the basics. Specifically, Exception Filters, Interceptors, and Guards came to the fore. Each added functionality and security layers, critical for robust application development. I'll be diving deeper into their specific implications and use-cases in future technical discussions.

## Full Structure

![backendSummary](/posts/Dev/backendSummary.png)

Our backend architecture differentiates between data storage and serving mechanisms, with MongoDB primarily handling unprocessed data storage and PSQL dedicated to serving client requests. To ensure seamless data transition between these systems, we employ regular cron jobs for consistent synchronization from MongoDB to PSQL. This weak coupling not only ensures that clients consistently access the most up-to-date data but also provides the flexibility to scale each component.

## Collaborating Using Postman

During the system's development phase, the need for rapid API prototyping became evident. Postman, with its capabilities for quick API endpoint testing, proved invaluable for iterative development. Recognizing the importance of alignment between frontend and backend development, we introduced a standardized JSON format specifically for the main page. 

![PostmanStruct](/posts/Dev/PostmanStruct.png)

This served as a clear contract for both teams. With such a format in place, frontend developers could simulate data interactions, allowing them to proceed with UI/UX development in parallel with API development. This collaborative approach will accelerate our overall project timeline.

![postmanUsage](/posts/Dev/postmanUsage.png)

## Conclusion

This project has provided me a comprehensive look into the complexities of our backend infrastructure and the nuances of the development. While I spearheaded most development and game research, our CTO's guidance was instrumental in refining our approaches and ensuring we stayed on the right track. I am learning a lot from our team, and I am glad to be able to work with them. I'll be elaboring more useful insights in my later posts soon. Stay tuned for more insights and technical breakdowns!