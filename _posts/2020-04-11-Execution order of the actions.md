---
layout: post
title: Actions' execution order
typora-root-url: ../..
---

**Disclaimer**: this is the first post about game design, a subject which I do not have any idea.

Big news, I have discovered that my game has two big problems. No more spoilers, let's see which are the problems.

## The problems

### Problem 1:  Imperfect information

I knew that it was a matter of time to face this problem. One of the main features of my game, the fact that players execute their actions simultaneously, has a big disadvantage: it does not allow the player to have perfect information about the game, so some of the decisions the player takes have to be based on guesses. I know, it does not seem like a problem, in a lot of games imperfect information takes an important role, but in this kind of game that quirk can create unpleasant situations.

Imagine this situation: there is just one player in each team. Both players are in a really small room, so both of them can reach each other with ranged attacks. One of them just has 1 point of life, has one skill that allows to protect himself for a round and he knows that in the next round the other player will attack him. What do you think he will do? He will just protect himself over and over, always forcing the other player to miss the attack. Of course this is an stupid example, but this same problem can have bigger implications in our game.



### Problem 2: Movement

If we analyse how the turn system in most of the tactical turn-based strategy games work we will discover one common thing: their turns are divided into two steps: movement and attack.  Examples of games that use this system are final fantasy tactics, Wargrove, Wakfu, Dofus, Card Hunter, Tactics Ogre, Battle for Wesnoth, advance wars, Into the Breach etc. Maybe it is that way for a reason.

Imagine that that our player and the enemy have exactly the same range of attack of 2 and they are at a distance of 3. If you want to attack the other player first you need a turn to approach him and another one to attack him. The problem is that if we are the ones taking the decision to approach to our opponent he does not need to take this action so he has an extra turn. That can create situations where the best option for both players is to do nothing, and that is very undesirable.



### The new problem

I have decided to make another change to the game in order to improve its playability. The original idea was that in each turn the player will execute one action, but now I want each turn to be composed of 3 actions. This means that we have to decide the three next actions before the first one gets executed and once it gets executed we can't change the other two actions. The main reason to do this is that it requires a better communication and a better planning to be successful in the game. Now a longer term strategy is required and is crucial that all the players in the team are able to develop its role in it.

Why this is a new problem? The truth it is that this is not a problem by itself but if not handled properly it increases a lot the information entropy.

> The information entropy, often just entropy, is a basic quantity in information theory associated to any random variable, which can be interpreted as the average level of "information", "surprise", or "uncertainty" inherent in the variable's possible outcomes.

Basically makes harder for the player to predict the possibles outcomes of its actions and by doing so does not allow the player to create complex strategies.



## The solution

There are many solutions to these problems, but I would like to keep the most important features of the game: simultaneous execution of actions and need of collaboration to archive shared goals.

After thinking about which is the best option I have decided that instead of one round for both teams, I will split the round in two, having one round for each team. This means that, during the decision making of our actions, all of the changes in the game will depend on the actions of our team. This encourages, even more, the need for good communication. Now all the information is accessible to us, either watching the map or talking with our teammates and knowing about which actions they are going to take.

This solution does not solve completely the problem of the movement, but with a good map design and set of abilities this should be more than enough to make the game fun to play.