# Prerequisites

This beginner's guide will walk you through creating a skeleton project that runs natively in the game's process.

First, you'll need to set up an appropriate developer environment.

::: warning
This guide is for Windows. There is currently no documentation for Linux or Android development.
:::

## IDE Setup

We recommend you use [CLion](https://www.jetbrains.com/clion/), a free (for non-commercial) C++ IDE.

### Creating a Visual Studio Toolchain

CLion's default toolchain, MinGW, is not ABI-compatible with Minecraft.

You'll need to install [Visual Studio](https://visualstudio.microsoft.com/vs/) to provide the required build tools. Install the `Desktop development with C++` package. 

::: info
You can also use Visual Studio as an IDE, but the beginner topics will assume you are using CLion. We are just installing it to provide the build tools to CLion.
:::

In CLion, go to **Settings** > **Build, Execution, Deployment** > **Toolchains**.
Click the plus icon, and select **Visual Studio**.

![Add Toolchain](/beginners-guide/prerequisites/add-toolchain.png)

Click the up arrow near the plus icon to move the new toolchain up until it's at the top, and click **Apply**. 

Now you have a Visual Studio toolchain, which can build code that is compatible with the game's ABI.

## DLL Injector

Unlike addons, a native mod directly interfaces with the game's memory and code. 
This is typically done inside the game's process. 
To get your code running inside the game's process, you will build your code into a `.dll` file and inject it into the game with a DLL injector.

For beginners, we recommend [Fate Injector](https://github.com/fligger/FateInjector/releases).

