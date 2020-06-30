---
layout: post
title: Client-Server communication
typora-root-url: ../..
---

Okay, this is way harder than I thought. But let's solve the problem one by one.

## What I want to do

This diagram shows more or less which the idea:

![](./tfgblog/assets/images/sequenceDiagram.svg){:.img}



Now I will try to explain with more detail what is going on there:

1. The map reads a file with all the configuration (entities, IP, map etc.) then it sends the configuration of each client.  It also sends the map in the same message.
2. The client replies to make sure that the connection is working. If in the future we want to allow the clients to participate in the configuration we would use this reply to send it to the server.

Now the game loop starts. It is composed of two turns.

3. All the members of the team 1 decide their actions and send them to the server. It is possible that the time ends and some or all members haven't send anything.
4. The server creates a new map based on the previous map and the actions that have received from the players. 
5. The server sends the new map and the actions taken by the team 1 to both teams

Then is the turn of the team 2, which follow the same procedure.



## How I am going to do it

### Serialisation

When I think about serialisation between python programs one library cames to my mind: pickle. Or Cpickle if we want to do the things faster. Since all the code will be written in python and the classes used to store the information are similar in the client and the server it seems to be the obvious choice. Then I saw this blog post [Don't Pickle Your Data](https://www.benfrederickson.com/dont-pickle-your-data/).

I also discovered that pickle was not that good of an idea since I do not want to share all the information. For example, the entities have a parameter that is the list of actions they can perform. Maybe I don't want this information to get shared with the other team to make the game more interesting.

And what if in the future I want to change the language of the AI client because I want to try some lisp library? Yes, interoperability may be another problem.

And if I want to store a log of the matches? I need to organise the information in a human-readable format to store it. 

JSON solves all these problems, so I am using JSON.

### Message

Okay, now we know that we want to send the information using JSON, but **which information**?

The **information that the map has to send** to the clients is:

- Lists of the actions ID that all the entities have taken. Of course, the list of all the members of one team will be empty.
- Map of the entities.
- Information about each one of the entities:
  - Name
  - ID
  - Life
  - Maximum life
  - Mana 
  - Maximum mana
  - Possible actions List
  - Taken actions list



The **information that the client has to send** to the map is:

- List of actions ID that the entity executes.



Okay, now we know what we want to send, but in **which structure**?

**The client message** is quite simple: a ordered list of triplets that contain the ActionType ID, the coordinate X of the destination and the coordinate Y of the destination. 

**The server message** requires some changes. The information is structured the way it is in the server for good reasons, but since the idea is that the server and the client are completely independent I think that the best option is to organise the information in a more natural way. That means that now instead of having a matrix of entities ID now we are going to store the full entity in the matrix, some information will be omitted and the actions will follow the same format that is used for the client message. For example, a map with two entities of different teams, each one in one starting in one corner of the map, after one round would look like this:

```json
[
	[{},{
        "name": "player1",
        "ID": 1,
        "life": 8,
        "max_life": 8,
        "mana": 4,
        "max_mana": 4,
        "possible_actions": [1,2,4,5,6],
        "taken_actions": [[1,1,0],[1,2,0],[2,2,2],[1,1,0]],
    ,{}],
	[{},{},{}],
	[{},{},{
        "name": "player2",
        "ID": 2,
        "life": 6,
        "max_life": 6,
        "mana": 6,
        "max_mana": 6,
        "possible_actions": [1,2,3,6],
        "taken_actions": [],
    }]
]
```

Reading this JSON structure we can know that it was the turn of player1 (because its "taken_action" list is not empty), and knowing that 1 is the actionID for move and 2 is the actionID for some kind of attack we can read that:

1. Player1 has moved to (1,0)
2. Then it has moved to (2,0) to have player2 in the line of sight
3. Player1 has attacked to the player2 position 
4. Player1 has moved to (1,0) to avoid being in the line of sight of player2.



### Broker

What I want to do is not that complicated to do with sockets, but by using a broker solves some problems that otherwise we should solve on our own.

The two options that I have evaluated are ZeroMQ and RabbitMQ. I have decided to use RabbitMQ since it seems that is easier to use and I do not need the extra speed that ZeroMQ offers.



## Conclusion

Before continue theorizing about how to implement the communication I want to create the serialisation and the unserialization functions for the server and client message. After that I will try to implement a isolated simplified version of the communication protocol which I can use to find problems. When I have something that is good enough I will implement it in the project.