---
layout: post
title:  "Devlog for Living World #1"
date:   2019-06-27 20:00:17 +0300
categories: development incremental living-world
---
## A little backstory

Since recently, I have been working on a hobby project in my free time - an incremental game, for now dubbed "Living World" (Didn't spend much thought on it).

Last week I made a [post about it on reddit](https://www.reddit.com/r/incremental_games/comments/c2y8qk/developing_a_lifesimulation_idle_game_looking_for/),
asking for opinions and ideas, and got a lot of useful information. That was quite inspiring,
so I decided to try and document my progress in this blog and see how it goes.

## Tidying up

First thing on my TODO list was to tidy up the code and project structure which was quite messy.
For example client-side rendering code in JavaScript was mostly copy-pasted from [Pixel Art Editor project from EloquentJavascipt](http://eloquentjavascript.net/19_paint.html)
(It's a great free book to learn JS by the way). So I was cutting off a lot of unneeded code and re-writing the rest.

On server-side the code also did not look very good, since I was (and still am) learning `Nim` language as I develop the game.

In short as a list:
* A lot of code cut and re-written
* Bugs = fixed
* Comments = added

## Some interactiveness

* Added a simple UI
* Implemented ways to interact with the world like changing tile to water and creating a sheep.
  I use these to test the game, not sure if they will still be in the actual gameplay.
* You can now pause and unpause the game

## Beautifying

* Little SVG images replaced plain circles to make the picture more beautiful.
  ![](/assets/img/living-world-icons/Sheep_001.svg)
  ![](/assets/img/living-world-icons/Grass_001.svg)
  ![](/assets/img/living-world-icons/Human_001.svg)
  Props to Niquari@deviantart
* I recolored desert tiles to be darker for better contrast with sheep.
* `setInterval(f, 500)` was replaced with `requestAnimationFrame` API to draw real-time.
  Server still ticks once per 500ms but client interpolates positions of moving sprites.

Before (Sorry for the black border top left):
![](/assets/vid/before.gif)

After:
![](/assets/vid/after.gif)

## Making the code public

* The source is now public on GitHub [here](https://github.com/CrowbarKZ/living-world)
* Feel free to take a peek and leave any feedback in the `Issues`
* Fork at will
* If you plan to submit a pull request, please contact me first (open an issue or pm me on reddit).
  So that we agree on proposed changes and you do not waste time working on something which would not end up
  in the project

## What is next?

In the upcoming weeks I plan to:

* Implement simple authentication
* Implement saving and loading game state
* Try [ECS](https://austinmorlan.com/posts/entity_component_system/) approach, since it seems to be a good fit for this type of games.
* Finally add humans to the game :)


## What about game mechanics?

No progress there yet, I want to build a solid foundation first so that the game is easy to extend in the future.
I keep the post with your ideas and a notebook with mine close at hand.

That's all, thanks for reading!
