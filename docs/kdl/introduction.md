# 4.1 Introduction to KDL
This section is going to introduce you to the _Kestrel Definition Language_ or more simply _KDL_. This is language
has been built to represent the data format and resource structures used by Kestrel.

## Hello, World!
We'll get started learning KDL with the stereotypical introductory script that is common for many to do when
learning a new programming language. Whilst KDL isn't a programming language in the strictest sense, it can still
perform various functions and calculations whilst assembling a game data file.

```kdl
// hello.kdl
@out "Hello, World!";
```

Create a new file and add the above contents to it. This is about as simple as a KDL script can be. The `@out` 
directive tells the KDL assembler to print the following string to the standard output. If we go ahead and run 
the script through KDL we get the following:

```sh
kdl hello.kdl
Hello, World!
```

But this isn't a particularly useful script... so let's move on to more useful basics.

## Creating our first resource
Let's dive straight into the next example. We'll go over what everything is doing shortly.

```kdl
// fruits.kdl
@import Macintosh;

declare StringList {
    new (#128, "Fruit Names") {
        Str = "Apple";
        Str = "Banana";
        Str = "Orange";
        Str = "Lemon";
        Str = "Melon";
        Str = "Grapes";
        Str = "Strawberry";
    };
};
```

The first line is a comment. Comments are always started using two forward slashes; for example `// this is a comment`. A comment is then 
terminated by a newline character. Comments are useful for adding descriptions into a KDL script to help act as reminders or explanations
of what certain things are in your script. In this case we have just used it to convey the filename for the example.

The next line is a module import directive. KDL includes three built in modules; `Kestrel`, `Macintosh` and `SpriteWorld`. Each of these 
modules provides various resource type definitions that are built into Kestrel itself.

The `Macintosh` module contains various resource type definitions for resources that existed within the Classic Macintosh system. In this
example we are after the `StringList` resource type (`STR#`).

We then move on to declaring our new resource. We tell KDL that we are about to start declaring resources of the `StringList` type by using
the `declare StringList` statement. If you wanted to declare resources of a different type, then you would substitute the `StringList` with
the name of the resource type in question. Following the declaration statement, we then have a serious of resource definitions, contained 
within braces, `{` and `}`.

Resources are defined using the `new` keyword. The `new` keyword expects to receive a couple of arguments, that tell it about the identity
of the resource. These arguments are:

1. Resource ID - This is provided as a _resource reference literal_. You can identify these by the proceeding hash (`#128`).
2. Resource Name - This is the name of the resource.

Once we have defined our new resource, we can once again open up a pair of braces and specify the contents of the resource itself. In the
case of the `StringList` resource type, it only provides a single field called `Str`, which expects to be given a string. However this field
is defined as part of a _repeating list_ and as such can be repeated up to 65,536 times. Each repetition of the `Str` field will be created
as an entry within the resulting resource.

Let's go ahead and build the script.

```sh
kdl fruits.kdl
```

We'll see a new file called `result.kdat` created alongside our `fruits.kdl` file. This is the default output file for KDL, if no explicit
file is specified.

If you are running on macOS then you'll be able to follow along with this next step, but don't worry if you are on Linux or Windows. This
is purely to visually illustrate the contents of the `result.kdat` file. You can use Andrew Simmons' excellent [ResForge](https://github.com/andrews05/ResForge)
tool to open Kestrel data files and take a look inside. ResForge is reading the data files using the same underlying Graphite library as 
Kestrel is.

<img src="https://github.com/Evocation-Games/kestrel-getting-started/blob/004bbf557437f40521cf0d61e6e113a50796e15e/docs/images/res-forge-fruit-names.png" width="766" />
<img src="https://github.com/Evocation-Games/kestrel-getting-started/blob/004bbf557437f40521cf0d61e6e113a50796e15e/docs/images/res-forge-editor-fruit-names.png" width="653" />
