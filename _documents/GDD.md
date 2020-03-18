---
layout: document
title: Game Design Document
typora-copy-images-to: ../assets
typora-root-url: ./
---

## Basic description of the game

The game is a tactical strategy game. The action takes place in a map where each entity occupies one discrete position. Each entity belongs to a team and has a set of abilities which has to use in order to archive certain goal that shares with all his team.

![Resultado de imagen de test image](./../assets/images/test.png){:.img width = 30%} 

## Map

The map has the shape of a rectangular orthogonal grid. In each position of this grid there can be three types of terrain:

- **Floor**: The entities can occupy this space and it does not blocks abilities.
- **Walls**: The entities can not occupy this space and it blocks abilities.
- **Holes**: The entities can not occupy this space and it does not block abilities.



## Entities

There can be three types of entities:

- **Passive entities**: Objects that occupy positions but do not execute actions.
- **Active entities**: Objects that occupy positions and in each turn execute actions.

- **Players**: Real players that occupy positions and can execute abilities. The main difference is between the active entities and the players is that the active entities do not take in to account the state of the game when executing actions.

Each entity has:

- An amount of HP
- A set of actions which can execute



## Actions

The actions have to be executed by an entity and have effect over a set of entities. The actions can be executed inside an area which position is defined in relation to the position of the entity that executes the action. Each action can involve zero entities, just the emitter, or the emitter and a set of receivers. 

There can be three types of actions:

- **Movement**: This actions just change the position of the player which executes the action.
- **Passive actions**: This actions are executed automatically in each turn
- **Active actions**: This actions need to be executed by an entity

Each action has:

- **Emitter**: The entity that executes the action
- **Receiving position**: The position which receives the action. We use a position instead of an entity because maybe there are more than one entity in that position or the action has an effect area. By just using a position then we can retrieve the set of entities inside the area.
- **Action**: describes what happens to the all the involved entities.



