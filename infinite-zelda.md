# Infinite Zelda project
`2012-09-09`

Some years ago I started working on a project I call Infinite Zelda. It’s a lot like Infinite Mario Bros! just for The legend of Zelda. It’s basically a level generator. The goal is to create fun-to-play zelda-like dungeons.

A zelda-like dungeon consists of connected rooms with enemies, keys and doors, bombs and cracks, switches, gaps, one-way paths and what have you. The player has a wealth of items at his disposal to overcome the challenges of the dungeon, beat the boss and eventually rescue the princess. Yay!

Generating such a dungeon is difficult. Especially a fun-to-play one. Even if you leave out the story-telling. Let’s focus on the puzzle aspects. There are constraints that must be met to prevent player frustration:

    The dungeon must be solvable. There must be a way.
    The player must not get stuck. There must not be a total dead-end.
    Every item and obstacle should be essential. You don’t want more keys than doors.

In a series of posts I’ll explain what it takes to generate such dungeons. I’ll break down the challenges in detail and present my approach. I’ll show how multiple layers of abstraction help modelling each part. Over the course it will become clear how all of this is related to genetic algorithms, abstract syntax tree interpreters and formal verification.
