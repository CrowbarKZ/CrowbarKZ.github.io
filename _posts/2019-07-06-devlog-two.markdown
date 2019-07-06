---
layout: post
title:  "Devlog for Living World #2"
date:   2019-07-06 16:58:00 +0300
categories: development incremental living-world
---
## Previous logs

* [Devlog #1]({% post_url 2019-06-27-devlog-one %})

## A step back

The week 2 of development was focused on implementing authentication and save/load systems.

Everything was going well up until the point where save/load started working.
As I tried to load a save from previous day once I found out that it was taking way too long
(waited one minute before killing the process). The culprit was offline progress, it works like this:

1. Check the timestamp of last world update from save
2. Get time delta between now and said timestamp
3. Limit the delta to 1 day (1 day of offline progress max)
4. Convert the delta to amount of "turns" which would have happened in that time (1 turn per 500ms)
5. Process N "turns" to get the fresh game state

As it turned out, once the world has around 1000 sheep, processing a turn starts taking around 0.01s,
and a day of progress is around 100_000 turns, which would result in approximately 25 minutes of processing for
1 day of offline progress. During those 25 minutes the server application would not be responsive, and if there
were other players they would all get a 25-minute freeze if someone connected and started loading the offline progress.

Obviously, this was not acceptable so I started looking for a way to make processing a turn faster

## Optimizing

The most commond advice about simulating large number of entities I found on the internet was to not use OOP,
and instead decouple data from logic. So the first step was to re-organize the data in the game - I've implemented
some ideas from ECS approach, getting rid of many classes and replacing them with tuples and scalars.

As a result of this - many things broke and most of the development time was spent on fixing everything that broke.
But timings became much better currently tests show that procssing 1 day of offline progress is taking around 30s on my machine
(down from 30m), despite the world being 25 times larger.

Alas 30s of unreponsive processing is still unacceptable so re-working offline progress is still on top of my TODO list.

## New features

Even though most of the past week was spent re-writing the existing functionality, there are still some new features this week:

* Simple sign up endpoint, allows to register a new user.
* Simple authentication system - now client needs to supply authentication token with each request.
  The token is issued upon connection if client provides proper login and password. Then the token is used to
  determine which world data to return the client.
* The world now is bigger (256x256 cells instead of 50x50) and is also looping around, i.e. sheep can walk from cell 255,y to 0,y in 1 turn.
* Cells(or tiles) of the world are now represented by height (0-10000), instead of a type (water, land, desert).
  Low height values are rendered as water (and are impassable), high ones as mountains (with snowcaps!)
* Save/Load system is 50% there.

A peek at how it looks now:
![](/assets/vid/devlog2.gif)

## The code

New code is on `feature/ecs` branch in the repo

## TODO for the upcoming weeks:

* Speed up, re-think, or decrease the limit of offline progress
* Make save/load working
* Work on game mechanics (not sure what will be the priority here, perhaps adding humans or adding the first resource or something else)

That's all, thanks for reading!
