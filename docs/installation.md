# 1. Installation
In order to use the Kestrel Game Engine, you will need to install it on your local system. There are different
sets of instructions here, depending on what system you are running.

## macOS
In order to install the Kestrel tooling on macOS, you will need to have the following

- macOS 11 Big Sur or later
- [Homebrew](https://brew.sh)
- A Code Editor (such as [VS Code](https://code.visualstudio.com))

Once you have all of the above prerequisites sorted out, you will need to use Homebrew to "tap" the Evocation-Games
homebrew repository. Tapping is the terminology used by Homebrew to provide it with information about a source of
"Formulas" or software.

To do this you will need to run the following command:

```sh
brew tap evocation-games/kestrel
```

This may take a several seconds to complete, and once it is done you will be able to install the kestrel tooling. The
tooling consists of three separate bits of software, `kdl`, `kdtool` and `kestrel`. We will need to install all of these.

```sh
brew install kdl kdtool kestrel
```

Congratulations, you now have the kestrel tooling installed and ready to use.

## Ubuntu
TBC

## Windows
TBC