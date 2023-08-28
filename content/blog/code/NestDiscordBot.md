---
title: "Discord Bot Development story"
date: 2023-03-08
draft: false
github_link: "https://github.com/Halfgear/NestDiscordBot.git"
author: "HwiJoon Lee"
tags:
  - NestJs
  - Discord
  - Docker
  - PostgreSQL
image: /projects/Bot_github.png
description: "Elaboration of my side project"
toc:
---
## Introduction 
Software engineering is a rapidly changing field. Every day, new tools and technologies are being released that can help developers more efficiently create and manage applications. As a software engineer, it can be difficult to keep up with all the changes and ensure that you are staying up to date with the latest best practices. In this blog post, I will share my experience reaching out to my [favorite league data analysis youtuber](https://www.youtube.com/channel/UCqI5lyTpC79pOy2D-VXAMdA)'s company and apply my knowlege and skill out side of class.

![ps spectator](/projects/ps.png)

## How I reached out to them
After completing the basic Computer Science classes, I looked for interesting side projects to join. I sent an e-mail to one of my favorite League YouTuber expressing my interest to join any proejcts and my desire to collaborate. To my surprise, the CEO responded and suggested that I start by helping with management of Discord community called NoTrollz for league of legend friendly matchs between clubs first. Then see if we could further develop something once we got to know each other better. I was thrilled to have the opportunity to work with a real-world company and acquire some valuable experience.

## Working on the Project
My recent project was to create a discord bot that manages user's roles and teams. The goal of the project was to learn Typescript and NestJs that would make the bot to be accessible to company's current work environment. One of the biggest challenges I faced was learning how to learning new language and framework with limited amount of documents. These tools are relatively new and I had no prior experience working with them. Despite the challenges, I was ultimately able to create a simple bot that automated the chore for the server manager.

![docker+postgresql](/projects/docker+postgresql.jpeg)

## Continuing Education 
After the first part, I had a meeting with a senior developer from the company I'm working with. During the meeting, we discussed the current feature and the senior developer suggested to create a user data base and develop a more advanced automation bot. 

The automation of the feature could not be done solely with Discord's features and a database was necessary to handle user's roles and teams for extendability and sustainability. To match the company's existing system, I learned and implemented Docker Compose and PostgreSQL. To ensure my plan of action would work, I conducted research and experimented with the new technologies before implementing them into the project.

## Dockerizing the bot
To begin with, I created a devcontainer with psql database in VS code, which allowed me to develop and test my bot in a containerized environment. Since this bot will run under AWS in future, I had to create the containerized environment to minimize unexpected bug. This was a crucial step in ensuring that my development environment was consistent across different machines.

Next, I built a data table using TypeORM Entity. When building the data table for my Discord bot, I took into consideration the potential future development plans and added data types that I would need from users.
```
@Entity()
export class notrollz_entity extends BaseEntity {
    @PrimaryGeneratedColumn("uuid")
    uuid: string

    @Index("discord_id", { unique: true })
    @Column({ type: "varchar" })
    discord_id: string

    @Column({ type: "varchar" })
    discord_tag: string

    @Column({ type: "varchar" })
    summoner_name: string

    @Column({
        type: "varchar",
        default: ""
    })
    team_name: string

    @Column({
        type: "boolean",
        default: false
    })
    team_catain?: boolean

}
```
One issue I encountered while building the data table was with the formatting of the Discord IDs. To resolve this, I used the uuid generator from Node.js to generate unique IDs for each user and gave the unique property to discord_id to prevent duplicate user registration on the database. By creating a well-structured data table with appropriate data types and using a unique ID generator, I was able to ensure that my Discord bot could accurately and efficiently store and retrieve user data. I then sent a query from my nestjs app with typeorm to qsql in the devcontainer to test the functionality of data table.

<img src="/projects/psql_command.PNG" style="display: block; margin-left: auto; margin-right: auto; width: 97%; height: 97%;"/>


After completing the initial development of the bot, I made the nestjs app into a Docker image and included it in a docker-compose file. This allowed me to easily deploy the bot to different environments and delegate the bot service to AWS instead of local computer. I also built a connection to psql in the new image with conscious volume mount, which was necessary for persisting data. When the connection was successful, I was very happy with my progress.

![discord_usage](/projects/discord_usage.PNG)

## Conclusion
Overall, my experience working on this project was very rewarding. In a short amount of time, I was able to apply my knowledge of Object Oriented Design and acquire new experiences with docker containers, postgres, and javascript. Continuous education is an important part of staying up to date in the ever-changing world of software engineering and this project gave me the great lesson. By researching and experimenting with the latest tools and technologies, I am looking forward to create an advanced bot that improved the workflow of the community management of the company.
