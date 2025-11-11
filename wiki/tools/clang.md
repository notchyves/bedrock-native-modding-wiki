# Clang
Clang an alternative compiler to MSVC based on LLVM. For advanced users, Clang is recommended over MSVC for its superior code analysis and code generation, at the cost of increased compilation time. What makes Clang most appealing is `clang-cl`, which provides an MSVC-like front end to the compiler, effectively acting as a drop-in replacement for MSVC.

When configured through `clang-cl`, Clang generates code that is mostly ABI-compatible with MSVC, which makes it viable for Bedrock mods.

## IDE Setup
[JetBrains' guide for CLion](https://www.jetbrains.com/help/clion/quick-tutorial-on-configuring-clion-on-windows.html#clang-cl).

[Microsoft's guide for Visual Studio](https://learn.microsoft.com/en-us/cpp/build/clang-support-msbuild?view=msvc-170).

## ABI Differences
Clang generates different type names than MSVC. This causes EnTT to generate different hashes for component types, which means EnTT won't be ABI-compatible with Minecraft on Clang out-of-the-box.