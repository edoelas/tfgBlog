---
layout: post
title: First Post
typora-root-url: ../..
---

This blog will be used to document my Bachelor's Thesis and this is the first post. At the top of the home page there are some important documents related to the development of the game (GDD, AI ideas, structure of the project etc.)

This post should have been done long time ago, so right now there are a lot of things to explain. I will try to make a briefly introduction to the project and explain it's actual state and how I would like it to evolve.

## The project

The project, is about the development of a proof of concept of a game. Which kind of game and its details are explained in the GDD. Due to the nature of this game there are some challenges that are different from other games. Most of these challenges are explained in the "Artificial Intelligence" and "Graphics" documents.

## Weird things about the game

There are some things that make this game kind of special:

- All **the game is executed in the server**, so you can use different clients to connect to the server. In fact, the server do not differentiate between AI an human players since the only thing it sees are messages.

- **All the actions are executed at the same time**: there are rounds, but in each round all the actions are executed at the same time. This means that the execution of one action can interact with the other one. For example, if two players decide to move to the same position what will happen is that both of them will loose a turn. Or it can also be introduced a way of combining two actions in order to make a third, more powerful action. In the other tactical games, since the execution of actions is sequential, this kind of behaviour could never happen.
- It uses **evolutionary algorithms** to improve the evaluation function.  

## Actual state of the game

Today I have just finished the basic structure of the server. yes, it does not seem a lot but I have taken a ton of time to develop this part since is the most important part of all the project. The graphical client, the AI client and the server will use more or less the same code, so it is really important to get it right from the beginning. In fact, to make any change to this code in the future would mean to have to change a lot of things, maybe one third of the full project (I'm kind of an expert in starting project without any kind of planning, so I know what I am talking about ).

I have already implemented some advance features like long range attacks which have an area effect, and I have to say that it is really easy to do. Things just work as them should, the code is clean and easy to maintain, the way it works logical etc. For being able to do this I had to learn a bit about design patterns (that's one of the reasons why it took so long) and in the code there are implemented modified versions of the Observer and Flyweight pattern designs and the strategy pattern designs. There will be a document explaining this topic with more detail.

About all the other parts of the game, I have an idea in my mind of what do I want to do but there are still lots of decisions to take and problems to solve.

## How the game should evolve

The MVP (minimum valuable product) is more or less clear. If everything goes as planned I should be working in the graphical client in about a week. It has been proven that I'm not able to estimate how much time things take, but even with that I will say that it will take me about 20 working days to develop the graphical client and once I have finished that I will have all the time in the world to focus on the AI.

There are still some design decisions that have to be made about how the client and the server communicate:

- Should I use Cpickle or json to serialize the data? 
- Should I just use python sockets or maybe something like [rabbitmq](https://www.rabbitmq.com/) ?
- Which data should I send and receive?

This decisions are supposed to be taken tomorrow, even I am quite sure that it does not matter what I decide it will change in the future. 