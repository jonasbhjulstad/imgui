cmake_minimum_required(VERSION 3.27)
project(imgui)
set(PROJECT_VERSION 1.82)
include(cmake/CPM.cmake)
CPMAddPackage("gh:TheLartians/PackageProject.cmake@1.6.0")

include(FindVulkan)

add_library(imgui
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_widgets.cpp
    imgui_tables.cpp
    backends/imgui_impl_vulkan.cpp
)

target_link_libraries(imgui PUBLIC Vulkan::Vulkan)

target_include_directories(imgui PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
    )

set(PROJECT_NAMESPACE imgui)
packageProject(
  # the name of the target to export
  NAME ${PROJECT_NAME}
  # the version of the target to export
  VERSION ${PROJECT_VERSION}
  # a temporary directory to create the config files
  BINARY_DIR ${PROJECT_BINARY_DIR}
  # location of the target's public headers
  INCLUDE_DIR ${PROJECT_SOURCE_DIR}/
  # should match the target's INSTALL_INTERFACE include directory
  INCLUDE_DESTINATION include/${PROJECT_NAME}-${PROJECT_VERSION}
  # (optional) option to install only header files with matching pattern
  INCLUDE_HEADER_PATTERN "*.h"
  # semicolon separated list of the project's dependencies
  DEPENDENCIES "Vulkan 1.2.162"
  # (optional) create a header containing the version info
  # Note: that the path to headers should be lowercase
  VERSION_HEADER "${PROJECT_NAME}/version.h"
  # (optional) create a export header using GenerateExportHeader module
  EXPORT_HEADER "${PROJECT_NAME}/export.h"
  # (optional) install your library with a namespace (Note: do NOT add extra '::')
  NAMESPACE ${PROJECT_NAMESPACE}
  # (optional) define the project's version compatibility, defaults to `AnyNewerVersion`
  # supported values: `AnyNewerVersion|SameMajorVersion|SameMinorVersion|ExactVersion`
  COMPATIBILITY AnyNewerVersion
  # (optional) option to disable the versioning of install destinations
  DISABLE_VERSION_SUFFIX YES
  # (optional) option to ignore target architecture for package resolution
  # defaults to YES for header only (i.e. INTERFACE) libraries
  ARCH_INDEPENDENT YES
  # (optional) option to generate CPack variables
  CPACK YES
)
#install header in the backends folder
install(DIRECTORY backends/ DESTINATION include/${PROJECT_NAME}-${PROJECT_VERSION}/backends FILES_MATCHING PATTERN "*.h")