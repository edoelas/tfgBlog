---
layout: document
title: Game Design Document
typora-root-url: ./
---

## Introduction to the game

The game is a tactical strategy game. The action takes place in a map where each entity occupies one discrete position. Each entity belongs to a team and has a set of abilities which has to use in order to archive certain goal that shares with all his team.

The next image should be useful to have a basic idea of which kind of game it is:

![Example of tactical game](./../assets/images/DX3FtRp.jpg){:.img} 



But our game has two big differences compared to other tactical strategy games:

1. **Our actions are ruled by the time**: this means that we will be able to execute one action every certain amount of seconds. This is the reason why I have decided to implement an action buffer.

   Imagine that we want to move 4 tiles in the same direction. We can press the movement button, wait for 3 seconds so the cycle finishes, pres again the button, wait for another 3 seconds and so on. Or with the action buffer we can press 4 consecutive times the movement button and all this actions will be stored in it. In each cycle the next action in the buffer will be executed. This allows the user to focus more in the strategy and less in the timing.

2. **All the actions are executed at the same time**: instead of taking turns to execute the actions in our game all the actions are executed together in two sub-cycles: ability and movement.



## Explanation of one game

Now I will try to make a really detailed explanation of how a game should be developed. With this I hope that there is no doubt about how the game works:

1. First we open our graphical client and get connected to a game

2. Once the connection has been done we will see the representation of the map and some other UI elements that give us information about the match.

   ![](./../assets/images/game1.svg){:.img}

3. We will have some seconds to design our strategy and decide our first movements. We have decided that the first thing we want to do is move 1 tile to the north and then attack the enemy that is there, so we add this actions to the action buffer.

   ![](./../assets/images/game2.svg){:.img}

4. The first cycle ends and our first action gets executed, but the enemy also has decided to move. Both entities collide and the action does not get executed.

   ![](./../assets/images/game3.svg){:.img}

5. Now the enemy is not in the adjacent square (as we had planned it was going to happen) so we empty our action buffer and change the strategy. 

   ![](./../assets/images/game4.svg){:.img}

6. Our new strategy will consist in wait for him to approach us and then attack.

   ![](./../assets/images/game5.5.svg){:.img}

7. The first action (waiting) gets executed and the enemy approach us as planned.

   ![](./../assets/images/game5.svg){:.img}

8. We are able to attack the enemy but he also attacks us.

   ![](./../assets/images/game6.svg){:.img}

Of course the real game will be a bit more complex and interesting, but I think in this example we have been able to see some of the main features of the game: 

- Collisions of actions.
- Not being able to predict the moves of our enemies.
- The use of the action buffer.



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



## Game design problems

### Execution of all the actions at the same time

Right now this is the only design choice that worries me. Since you don't know the action of your opponent when you are deciding your action lots of action choices will be have to based on guesses. This can be fun or can make the user feel like that the game is a matter of luck. These are some alternatives:

- **Each team executes the actions in different turns**: by doing this we make sure that the chosen action is based on a complete information about the game.

  <u>Advantages</u>: 

  - Easy to implement.
  - The original idea of the game prevails.

  <u>Disadvantages</u>:

  - One team has one more turn than the other.

- **Non synchronised actions**: each action has a different duration. For example, if you want to heal a friend it takes 5 turns, so the enemy, knowing what you are doing, can attack you since it knows that you will try to hold the position for 5 turns.

  <u>Advantages</u>: 

  - More complex game (not sure if it is an advantage)

  <u>Disadvantages</u>:

  - Harder to implement
  - AI more complex

For the MVP I will implement the first solution.



### Other ideas

- **Multiple actions from the action buffer executed in a row:** maybe it is possible make the game more focused in the strategy and less in the action if multiple actions from the action buffer are executed in a row. This will require a better communication between the members of the team and the need for longer term strategies.

