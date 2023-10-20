# 3. Building a simple game
In this section we are going to explore the process of creating a very simple "game". I use the term game very
loosely here, as it won't really have much in the way of game play.

Let's take a look at what we hope to accomplish here.

1. Show a simple entity on screen.
2. Show a simple sprite on screen.
3. Navigate between scenes.
4. Handle keyboard input.
5. Handle mouse input.
6. Draw text to the screen.

## Setting up...
To get started, let's create a new project. We covered this in more detail in the previous section, but here is the command
we need to run.

```sh
kdtool create-project game MySimpleGame scene:Stage
```

We just want the most basic project we can get with a simple "Stage" scene created by default.

Make sure that everything was created correctly by running:

```sh
cd MySimpleGame
kdl -o build/Game.rsrx project.kdlproj
```