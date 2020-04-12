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



## Explanation of one game

1. 



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

