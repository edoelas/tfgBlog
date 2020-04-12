---
layout: post
title: Actions' execution order
---

**Disclaimer**: this is the first post about game design, a subject which I do not have any idea.



Okay, big news, I have discovered that my game has two big problems but I just have to solve one. No more spoilers, let's see which are the problems.

## Problem 1:  Imperfect information

I knew that it was a matter of time to face this problem. One of the main features of my game has a big disadvantage: it does not allow the player to have perfect information about the game, so some of the decisions the player takes have to be based on guesses. I know, it does not seem like a problem, in a lot of games imperfect information takes an important role, but in this kind of game that quirk can create unpleasant situations.

For example, What happens if I attack to one position and the enemy decides to move? The enemy will always be able to dodge all of our attacks because we are not able to know where he will be.

## Problem 2: Movement

If we analyze how the turn system in most of the tactical turn-based strategy games work we will discover one common thing: their turns are divided into two steps: movement and attack.  Examples of games that use this system are final fantasy tactics, Wargrove, Wakfu, Dofus, Card Hunter, Tactics Ogre, Battle for Wesnoth, advance wars, Into the Breach etc. Maybe it is that way for a reason.

What happens if the enemy decides to run because he is afraid to die? He will always be faster since we need an extra turn to attack. Each time we try to attack en we fail he is getting further and further.



## The relation between the two problems

We could think that, even with a different way of handling the movement, our game could work. And we also can think that there are a lot of good games which use the non-perfect information in its favour. But when in our game we combine those two things we generate a high information entropy, which is not good for strategy games.

By just solving one of those two problems our game could be playable again. I have decided to solve the non-perfect information problem by making the game deterministic. Later I will explain how, but first I would like to explain to you the reason why I need to solve this problem.



## The bigger problem

Imagine that suddenly I decide to make a change in the game that barely changes the game implementation but changes quite a lot the gameplay... Imagine that the change makes the problems way worse...

Thant change is that now the actions will be executed in rounds of three. This means that we have to decide the three next actions before the first one gets executed and once it gets executed we can't change the other two actions. Now the game is not playable.

Why would someone want to make that change? Because maybe that person has remembered that this was probably the second most interesting feature of the game that has served as an inspiration for his game. 

By the way, one of my main inspirations is Castle hustle from Airconsole. Here is a gameplay: https://www.youtube.com/watch?v=lsHv-es-QKU



## The solution

The solution is simple: instead of one round for both teams, we will have one round for each team. This means that, during the decision making of our actions, all of the changes in the game will depend on the actions of our team. This encourages, even more, the need for good communication. Now all the information is accessible to us, either watching the map or talking with our teammates and knowing about which actions they are going to take.



## The final decision

Yes, I am going to change. I think the game will be better that way. It will be more focused on communication and long term strategy than in fast decision making and action. 