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

<p align="center">
    <img src="https://github.com/Evocation-Games/kestrel-getting-started/blob/004bbf557437f40521cf0d61e6e113a50796e15e/docs/images/res-forge-fruit-names.png" width="766" />
    <img src="https://github.com/Evocation-Games/kestrel-getting-started/blob/004bbf557437f40521cf0d61e6e113a50796e15e/docs/images/res-forge-editor-fruit-names.png" width="653" />
</p>

## Literals & Syntax
Let's take a step back and start taking a look at the structure of KDL itself. We've now seen how it can be used to define data and build data files
accordingly. Before proceeding further, we need to take a look at the syntax of the language. This includes control structures and literals.

### Strings
String literals are one of the most basic aspects of KDL. A string is a sequence of text surrounded by quotation marks. For example:

```kdl
"this is a string"
```

There are some caveats with strings however. They do not support escape sequences, such as `\"` or `\n`, which are common in most languages. This is currently
a limitation of the KDL parser, and will be implemented at somepoint.

### Integers
Integer literals are another extremely basic aspect of KDL. Integers are whole numbers that can take several forms:

```kdl
// Stanard Integers
42
100
-32
-128

// Hexadecimal Numbers
0x80
0xDEADBEEF

// Percentages
50%
```

All types of integer ultimately get encoded exactly the same, but the representations can be useful for semantics and visually recognising
those values.

### Resource References
Resource references are a unique type of literal to KDL/Kestrel, and represent the identity of a resource. At there most basic they
simply represent a resource id, like so:

```kdl
#128
#1000
#10000
```

Each of these references are referring to resources of the _Global_ resource container and ignoring resource type. A `StringList (STR#)` resource with
an id of `128` or a `MacintoshPicture (PICT)` resource with an id of `128` would both be matched by the reference `#128`. Generally
unconstrained resource references should be avoided, and _must_ rely on something else to infer the resource type.

We can add constraints to a resource reference. There are two levels of constraint that can be applied. The first is a constraint on 
the resource type.

```kdl
// Format
#[ResourceType.]128

// Examples
#StringList.128
#MacintoshPicture.128
```

Both of these references refer to resources with an id of `128`, but they each have a different type constraint. Kestrel can use this
constrain to help identify the correct resource.

The next level of constraint builds upon the first (as in you must constrain the resource type, before this). This constraint works on _Containers_.
Resource Containers act as a method of grouping related resources together. This can be useful for preventing unintentional resource conflicts across
data files. We'll get into the exact mechanics of data files and resources in another section, but to briefly summarize; resources are loaded from
data files into a _Resource Manager_ within Kestrel. If a resource has the same resource type and id as an existing resource, it will override/replace
the existing resource. Sometimes this is the desired behaviour, particularly if you are trying to patch functionality in a game. However you may also
want to not worry about your content being accidentally broken by collisions with other data files.

This is done using containers. By default all resources are created into the _Global Resource Container_. We can specify a resource container in the
reference like so.

```kdl
// Format
#[[Container.]ResourceType.]128

// Examples
#CoreGame.StringList.150
#Expansion.StringList.150
```

By specifying the container, both of these resource references now refer to different resources.

### Resource Declaration
Declaring resources is central to actual adding data into a data file in the form of resources. We briefly looked at an example using `StringList`
earlier.

The basic structure of a declaration is...

```kdl
declare ResourceType {
    // Add resources here
};
```

You can also specify that all resources within the declaration belong to a container...

```kdl
declare Container.ResourceType {

};
```

Individual resources are then created by using one of the following...

```kdl
new (#id, "Resource Name") {
    // Specify fields and values...
};

override (#id, "Resource Name") {
    // Specify fields and values to change from the original...
};

duplicate (#source as #result, "Resource Name") {
    // Duplicate the source resource, and create use it to create the resulting 
    // resource with the specified fields and values changed
};
```

The most common method of creating resources is using the `new` keyword. This will create a brand new
resource.

`override` will check for an existing resource that is known to KDL, read it and change any fields that are
specified with new values and save the result to the output. Note that it does _not_ replace the original 
resource.

`duplicate` will check for an existing resource that is known to KDL, read it and change any fields in the same
way as `override` does. However it will save the result to the output with a new id as specified.

### Components

### Modules

### Variables

### Functions
