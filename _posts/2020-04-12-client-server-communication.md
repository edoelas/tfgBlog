---
layout: post
title: Client-Server communication
---

Okay, this is way harder than I thought. But let's solve the problem one by one.

## What I want to do

This diagram shows more or less which the idea:





Now I will try to explain with more detail what is going on there:

1. The map reads a file with all the configuration (entities, IP, map etc.) then it sends the configuration of each client.  It also sends the map in the same message.
2. The client replies to make sure that the connection is working. If in the future we want to allow the clients to participate in the configuration we would use this reply to send it to the server.

Now the game loop starts. It is composed of two turns.

3. All the members of the team decide their actions and send them to the server
4. The server creates a new map based on the previous map and the actions that have received. 
5. The server sends the new map and the actions taken by the team to all the teams

Then is the turn of the other team, which follow the same procedure.



## How I am going to do it

### Serialisation

When I think about serialisation between python programs one library cames to my mind: pickle. Or Cpickle if we want to do the things faster. Since all the code will be written in python and the classes used to store the information are similar in the client and the server it seems to be the obvious choice. Then I saw this blog post [Don't Pickle Your Data](https://www.benfrederickson.com/dont-pickle-your-data/).

I also discovered that pickle was not that good of an idea since I do not want to share all the information. For example, the entities have a parameter that is the list of actions they can perform. Maybe I don't want this information to get shared with the other team to make the game more interesting.

And what if in the future I want to change the language of the AI client because I want to try some lisp library? Yes, interoperability may be another problem.

And if I want to store a log of the matches? I need to organise the information in a human-readable format to store it. 

JSON solves all these problems, so I am using JSON.

### Broker

What I want to do is not that complicated to do with sockets, but by using a broker solves some problems that otherwise we should solve on our own.

The two options that I have evaluated are ZeroMQ and RabbitMQ. I have decided to use RabbitMQ since it seems that is easier to use and I do not need the extra speed that ZeroMQ offers.



[TO BE CONTINUED]