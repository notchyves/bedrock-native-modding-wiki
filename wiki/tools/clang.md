# Clang
[Clang](https://clang.llvm.org/) is an alternative compiler to MSVC based on LLVM. For advanced users, Clang is recommended over MSVC for its superior code analysis and code generation, at the cost of increased compilation time. What makes Clang most appealing is `clang-cl`, which provides an MSVC-like front end to the compiler, effectively acting as a drop-in replacement for MSVC.

When configured through `clang-cl`, Clang generates code that is mostly ABI-compatible with MSVC, which makes it viable for Bedrock mods.

## IDE Setup
[JetBrains' guide for CLion](https://www.jetbrains.com/help/clion/quick-tutorial-on-configuring-clion-on-windows.html#clang-cl).

[Microsoft's guide for Visual Studio](https://learn.microsoft.com/en-us/cpp/build/clang-support-msbuild?view=msvc-170).

## ABI Differences
Clang generates different type names than MSVC. This causes EnTT to generate different hashes for component types, which means EnTT won't be ABI-compatible with Minecraft on Clang out-of-the-box.
| Compiler  | `entt::type_name<T>::value()`                | `entt::type_hash<T>::value()` |
|:----------|:---------------------------------------------|:------------------------------|
| MSVC      | `"class FlagComponent<struct OnGroundFlag>"` | `0x211F2DE1`                  |
| GCC/Clang | `"FlagComponent<OnGroundFlag>"`              | `0x062EEC98`                  |

Fortunately, there is a fix for this, provided by [@ZeroMemes](https://github.com/ZeroMemes):
```cpp
class IEntityComponent;

template <std::derived_from<IEntityComponent> Type>
struct entt::type_hash<Type> {
    [[nodiscard]] static consteval id_type value() noexcept {
        constexpr auto name = Type::type_name;
        return hashed_string::value(name.data(), name.size());
    }

    [[nodiscard]] consteval operator id_type() const noexcept {
        return value();
    }
};
```

With this fix, you will have to add an extra static member to your classes, here we can use ```hat::fixed_string``` from [libhat](https://github.com/BasedInc/libhat). This makes the hash match what it would be on regular MSVC, instead of what it is normally on Clang/GCC.

```cpp
template<typename T>
class FlagComponent : IEntityComponent {
public:
    static constexpr hat::fixed_string type_name
        = "class FlagComponent<" + T::type_name + ">";
};

struct OnGroundFlag {
    static constexpr hat::fixed_string type_name
        = "struct OnGroundFlag";
};
```
