---
layout: document
title: Software design

---

## Server class diagram

This is the class diagram tha I have chosen to follow for the project:

![mermaid-diagram-20200409134236](./../assets/images/mermaid-diagram-20200409134236.svg){:.img}

This is one of the most important parts in the project because, even if it does not belong to the AI or graphics, this code , with some modifications, will be used in the graphical client, AI client and server.

It has taken a lot of time and iterations to get this right and after using this structure for some time I think is the good one, but maybe it suffers some minor changes.

Three different design patterns (or modifications of those patterns) have been used here:

- **Observer**: since I want to have a representation of the map as a matrix (for making some calculations faster) but I also wanted to modify all the atributes of the entities from the Entity object I have decided to make the object map a subscriver of all the entities of the game. By doing so I am able to change the position of the entity changing the atributes of it and automatically the position will be updated on the map. Since there is just one subscriber the implementation is simpler.
- **Strategy**: 
- **Flyweight**: In this case it hasn