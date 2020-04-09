---
layout: post
title: First Post
---

This blog will be used to document my Bachelor's Thesis and this is the first post. At the top of the home page there are some important documents related to the development of the game (GDD, AI ideas, structure of the project etc.)

This post should have been done long time ago, so right now there are a lot of things to explain. I will try to make a briefly introduction to the project and explain it's actual state and how I would like it to evolve.

## The project

The project, as you may already know, is about the development of a proof of concept of a game. Which kind of game and its details are explained in the GDD. Because of the nature of this game there are some challenges that are different from other games. Most of these challenges are explained in the "Artificial Intelligence" and "Graphics" documents.

## Weird things about the game

There are some things that make this game kinda of special:

- All **the game is executed in the server**, so you can use different clients to connect to the server. In fact, the server do not differentiate between AI an human players since the only thing it sees are messages.

- **All the actions are executed at the same time**: there are rounds, but in each round all the actions are executed at the same time. This is the reason why we need an action buffer and a way to solve the conflicts (for example, when two players try to go to the same position there is a conflict)
- It uses **evolutionary algorithms** to improve the evaluation function.  

## Actual state of the game

Today I have just finished the basic structure of the server. yes, it do not seem that a lot, in fact it is not, but I have taken a lot of time to develop this part since is the most important part of all the project. The graphical client, the AI client and the server will use more or less the same code, so it is really important to get it right from the beginning. In fact, to make any change to this code in the future would mean to have to change a lot of things, maybe one third of the full project (I'm kind of an expert in starting project without any kind of planning, so I know what I am talking about ).

I have already implemented some advance features like long range attacks which have an area effect, and I have to say that it is really easy to do. Things just work as them should, the code is clean and easy to maintain, the way it works logical etc. For being able to do this I had to learn a bit about design patterns (that's one of the reasons why it took so long) and in the code there are implemented modified versions of the Observer and Flyweight pattern designs and the strategy pattern designs.

## How the game should evolve

Now MVP (minimum valuable product), the things that have to be implemented, the order in which it has to be done and how to do it is more or less clear. If everything goes as planned I should be working in the graphical client in about a week. It has been proven that I'm not able to estimate how much time things take, but even with that I will say that it will take me about 20 working days to develop the graphical client and once I have finished that I will have all the time in the world to focus on the AI.

There are still some design decisions that have to be made about how the client and the server communicate:

- Should I use Cpickle or json to serialize the data? 
- Should I just use python sockets or maybe something like [TODO] ?
- Which data should I send and receive?

This decisions are supposed to be taken tomorrow, even I am quite sure that it does not matter what I decide it will change in the future. 