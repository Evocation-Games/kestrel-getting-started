# 2. Creating a new project
In this part we will go over the basics of creating a new project for Kestrel. If you have not yet installed the Kestrel tooling, then please check out
part 1 of the guide first.

## kdtool
Kestrel project management is handled by `kdtool`. Consequently it is also the tool that we can use to create those projects. Let's take a look at
a command to produce a new project called **MySimpleGame**.

```sh
kdtool create-project game MySimpleGame
```

This will create a new directory in your current working directory called `MySimpleGame`, and it will have a number of files and directories added
automatically. The structure of it should look something like this:

```
+ MySimpleGame
    + scenes
        + ExampleScene
            + scripts
                ExampleScene.lua
            ExampleScene.kdl
    + scripts
        main.lua
    project.kdlproj
```

This default project is about the most basic Kestrel project that we can produce. It doesn't really do anything, but rather just provides the basic
scaffolding for us to use going forward.

We can build the project like so, using the `kdl` tool (Kestrel Definition Language).

```sh
cd MySimpleGame
kdl -o build/SimpleGame.rsrx project.kdlproj
```

This will produce a new `build` directory in the root of the project, and inside it will be the `SimpleGame.rsrx` file. This file contains the game
data all packaged up for use in Kestrel.

In order to test our simple game, we can run the following:

```sh
kestrel --game build/SimpleGame.rsrx
```

This should launch the Kestrel Test Player and use the specified game file as its primary game data. If all has worked correctly, you should a window
appear with the title "MySimpleGame" and nothing but a black fill. If this is what you're getting, then congratulations! You have successfully built
your first game in Kestrel.

## Configuring
Navigate back out of the `MySimpleProject` directory and remove it.

```sh
rm -rf MySimpleProject
```

We want to explore creating more complex initial projects. Whilst there is no requirement for creating an initial project beyond the simple setup used above,
it can be helpful for setting up some things more quickly. In this example, we'll create a project that displays an ImGui Dialog when the game is launched.

```sh
kdtool create-project game DialogExample "author:Your Name" dialog:Example
```

There are a couple more parts to this command over the previous simple example. Let's take a look at them.

- `"author:Your Name"`

  This tells `kdtool` the name of an author of the project. It is simple piece of metadata that will be added into the `project.kdlproj` file.
  You can also add the following bits of metadata in this way; `license`, `copyright`, `version`.

- `dialog:Example`

  This tells `kdtool` to add a dialog named `Example` to the project. Alternatively you could also use `scene:` instead. There are no limits
  on the number of scenes/dialogs that you can add to a project. The default scene/dialog presented will always be the first created.

Once again we can try building this and running it.

```sh
cd DialogExample
kdl -o build/Dialog.rsrx project.kdlproj
kestrel --game build/Dialog.rsrx
```

If everything has worked correctly, then you should have a kestrel test player open a window titled "DialogExample", with an ImGui dialog and console
present within it.