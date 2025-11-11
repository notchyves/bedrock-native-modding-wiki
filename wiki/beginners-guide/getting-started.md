# Getting Started

Now that you've installed the [Prerequisites](/beginners-guide/prerequisites.md), you're ready to make your first mod!

Start by creating a **New Project** in CLion, and selecting **C++ Library**. Set the **Language standard** to at least C++20, and the **Library type** to shared. This will make your code compile to a `.dll` file that can be injected into the game.

![New project](/beginners-guide/getting-started/new-project.png)

In the top right, you will see a dropdown labeled **Debug**. Select it, click **Edit CMake profiles...**, and make sure the **Debug** profile is using the Visual Studio toolchain you created.

![Debug toolchain](/beginners-guide/getting-started/profile-toolchain.png)

## Configuring CMake

Open `CMakeLists.txt`. It should look something like this:

```cmake
cmake_minimum_required(VERSION 3.31)
project(project_name_here)

set(CMAKE_CXX_STANDARD 20)

add_library(project_name_here SHARED library.cpp)
```

Above `add_library`, add the following:

```cmake
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreadedDLL")
```

This configures your DLL to use the release runtime library, which ensures ABI compatibility (TBA!!!!!!!!!!!!!!! ADD A LINK TO THE ABI COMPATIBILITY ARTICLE RN!!!!!!!!!!!!!!!!) with the game when building in debug mode. Without this, you wouldn't be able to use standard library types like `std::string` with the game!

## Writing DllMain

Right click `library.cpp` and `library.h` and click **Delete**. Right click the root folder in the project view, click **New** > **C/C++ Source File**. Name the file `dllmain.cpp`.

Add the following code to `dllmain.cpp`:

```cpp
#include <Windows.h>
#include <cstdlib>

BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpvReserved) {
    std::abort();
}
```

`DllMain` is the function that will be called by Windows when your mod is injected into the game's process. The call to `std::abort` will just crash the game to show us that the DLL was injected.

Now, update your `CMakeLists.txt` to compile `dllmain.cpp` instead of `library.cpp`:

```cmake
add_library(my_first_native_mod SHARED dllmain.cpp)
```

Finally, click the hammer icon in the top right of the IDE, and build the project! If all went well, you should see this:

![Sucessful build](/beginners-guide/getting-started/successful-build.png)

Now, inject the DLL into Minecraft. If your game crashed, that means it worked!