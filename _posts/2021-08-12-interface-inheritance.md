---
layout: post
title: Alternative to interface inheritance
permalink: 2021-08-12-interface-inheritance
---

I have been using interface extension in java seamlessly for years and never really thought about if it can be done different way. Recently I have experimented with an idea and it seems to be paying off. 

We all know that for classes composition over inheritance is a way to go. But what happens when we apply this to interfaces?

### Example 
Let's draw an example problem here. 
I do have three kinds of a record. The basic one only contains data and has no knowledge about its schema. 
Second is aware of the schema 
And the third one is just a specialization of the two above and represents a single record. 

Normally I would do 

```java
interface Entity {
	Collection<SomeData> data();
}

interface TraversableEntity extends Entity {
	TraversableEntity traverseTo(Property prop);
}

interface SingleRecord extends TraversableEntity {
	Identifier id(); 
}
```

This might not be the best design but it's typically what I have seen most of my career. 

You might also object that it's upon implementation to implement a combination of those interfaces so it does not make sense to extend them. BUT a SingleRecord must always be traversable in this case so I want to embed that information into the code. 

What I did differently this time is.

```java
interface Entity {
	Collection<SomeData> data();
}

interface TraversableEntity {
	Entity asEntity(); // this

	TraversableEntity traverseTo(Property prop);
}

interface SingleRecord {
	TraversableEntity asTraversable(); // this

	Identifier id(); 
}
```

The inheritance is gone but the information about SingleRecord being also traversable is still present. 

In the extension way, implementation of a SingleRecord would have to implement all methods and delegate them or reimplement them. 
In the latter one, it can return the delegated implementation or return a new one based on some condition. And it's just a single method instead of all the inherited ones thus reducing boilerplate code and reducing duplication. This is also useful when adding a new method to an interface that does not result in all the child interface implementors breaking. 

Also when testing it allows me to stub a simple interface and return null/throw an exception for the inherited one if its methods are not used.

So far this approach has passed some usability tests over time and seems like it's worth adopting more. 

