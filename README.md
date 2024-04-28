# Dear ImGui-Vulkan

Minimalistic CMake-configuration of Dear ImGui for glfw Vulkan

## CMake

```
CPMFindPackage(NAME imgui
    GITHUB_REPOSITORY jonasbhjulstad/imgui
    GIT_TAG master)
```

```
find_package(imgui REQUIRED)
target_link_libraries(<target> imgui::imgui)
```

## Include
```
#include <imgui/[imgui_header.h]>
```