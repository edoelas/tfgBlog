---


layout: document
title: Description of the game
typora-root-url: ./
---

## Introduction

The game is a tactical strategy game. The action takes place in a map where each entity occupies one discrete position. Each entity belongs to a team and has a set of abilities which has to use in order to archive certain goal that shares with all his team. The execution of the abilities is done in rounds of three separated for each team. There is a limited time for taking the decisions of which actions to choose.

 If you haven't understood anything don't worry, is normal, later we will see a simulation of one match and everything will be crystal clear.



## Inspirations for the game

I think is a good idea to understand the main points of the game to talk about which games have served as an inspiration and which ideas I have taken from each one of them 

### Castle hustle (airconsole)

![Castle Hustle](./../assets/images/Ct_gFsTXYAA2_zU.jpg){:.img}

- The actions are executed simultaneously.
- The player has to decide his 4 next actions and once the execution gets started he can't change them.
- Need for communication inside the team.
- Limited time for making decisions.



### Into the Breach

![Into the Breach](./../assets/images/maxresdefault.jpg){:.img}

- The user has perfect information about the game
- Simpler mechanics allow the user to develop better strategy



### Wakfu

![Wakfu](./../assets/images/wakfu-4.jpg){:.img}

- The user just control one entity.
- Each entity has his own set of abilities and properties (like in an RPG)



## Simulation of one game

**Disclaimer**: for some unknown reason the images have been exported with some graphical artefacts. Let's just imagine that the map is a perfect grid without missing lines and that all the lines are the same width.

For this simulation, let's assume that there are just two actions: moving and kicking. You can move to adjacent squares and if you get kicked you are dead.

This is the starting state of the game. Two players in each team. 

![](./../assets/images/sim1.svg){:.img}

The red team starts. Each player has to choose the three actions it will execute during the first round of the red team. They decide to try to get to the other team through the hole in the wall. The player 1 will get the central position and the player two will be just behind him. Let's see how the actions are executed:

![](./../assets/images/sim2.svg){:.img}

![](./../assets/images/sim3.svg){:.img}

Oh! Disaster! What has happened?! A bad communication in the team has created a collision between the two players. When this happens the action does not get applied, but there are still some actions left to execute. Remember that during the executions of the actions the players can't change them.

![](./../assets/images/sim4.svg){:.img}

![](./../assets/images/sim5.svg){:.img}

The rest of the actions are executed and the situation of the game ends like this. During the last action, the player 1 has tried to go through a wall. He has discovered how physics work the hard way and his action hasn't been applied. Now is the round of the blue team, which has a way better communication. Player 1 decides to go first to avoid the problem that the red team had.

![](./../assets/images/sim6.svg){:.img}

![](./../assets/images/sim7.svg){:.img}

![](./../assets/images/sim8.svg){:.img}

They have reached the position that they wanted but maybe that was not the best strategy. Now it is turn of the red team again and to avoid more conflicts they decide that the player 1 is the one that is going to do all the job. He will move in front of the player 1 of the blue team, kick him and take its position.

![](./../assets/images/sim9.svg){:.img}

![](./../assets/images/sim10.svg){:.img}

It seems to work, but now he is exposed to the player 2 of the blue team. The player two is really clever so it kicks player 1 of the read team and performs a tactical retreat.

![](./../assets/images/sim11.svg){:.img}

![](./../assets/images/sim12.svg){:.img}

Now all the player 2 of the blue team is to wait and hope that the player 2 of the red team is ~~stupid~~ not good at strategy games.

![](./../assets/images/sim13.svg){:.img}

It is not. The red player gets too eager about the victory and makes a big mistake. Now he is right in front of the blue player and can't do anything. Lets see if the blue player is clever enough to win this match.

![](./../assets/images/sim14.svg){:.img}

![](./../assets/images/sim15.svg){:.img}

Yes! The blue player is incredibly smart and manages to get the victory for his team.



## More detailed description

### Map

The map has the shape of a rectangular grid. In each position of this grid there can be an entity.



### Entities

The entities can be divided in: 

- **Passive or active**: Passive if do not execute actions and active if they execute actions.
-  **Blocking or non-blocking**: blocking if the avoid other entities to occupy this position or non blocking if they allow blocking entities (but no non-blocking) to occupy the position. 

Each entity has:

- An amount of HP
- A set of actions which can execute



### Actions

The actions have to be executed by an entity and have effect over a set of entities. The actions can be executed inside an area which position is defined in relation to the position of the entity that executes the action. Each action can involve zero entities, just the emitter, or the emitter and a set of receivers. Moving the player is also considered an action.

Each action has:

- **Emitter**: The entity that executes the action
- **Receiving position**: The position which receives the action. We use a position instead of an entity because maybe there are more than one entity in that position or the action has an effect area. By just using a position then we can retrieve the set of entities inside the area.
- **Action**: describes what happens to the all the involved entities.

Actions will also have cooldown (and maybe casting time).

### Effects

The effects are actions that affect in each turn one entity. For example, not allowing it to move or healing certain amount of life.



## Minimum Viable Product

In order to know if the idea is working as expected first I am going to develop the MVP. Quoting [Wikipedia](https://en.wikipedia.org/wiki/Minimum_viable_product):

> A **minimum viable product** (**MVP**) is a version of a product with just enough features to satisfy early customers and provide feedback for future [product development](https://en.wikipedia.org/wiki/New_product_development).

In our version of the MVP we will just have:

- Entities: just the blocking entities will be implemented. The implemented entities will be players and walls.
- Actions: a small set of actions (Move, throw, kick, heal) will be implemented. Actions won't have cooldown.
- Effects: There won't be any effect.

