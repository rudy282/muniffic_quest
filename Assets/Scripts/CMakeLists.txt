cmake_minimum_required(VERSION 3.8)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif()
set(CMAKE_SHARED_LINKER_FLAGS_DIST "")
add_subdirectory("${CMAKE_BINARY_DIR}/../../../../../Muniffic-ScriptCore" "${CMAKE_BINARY_DIR}/../../../../Muniffic-ScriptCore")
#set(PROJECT_NAME "Example" CACHE STRING "name of project")
project(${PROJECT_NAME} LANGUAGES CSharp)

# Define configurations
set(CMAKE_CONFIGURATION_TYPES "Debug;Release;Dist" CACHE STRING "" FORCE)

# set(MunifficRootDir "../../../../")

# Set the output directories for binaries and intermediate files
add_library(${PROJECT_NAME} SHARED)
set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY    ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    RUNTIME_OUTPUT_DIRECTORY_DEBUG    ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    RUNTIME_OUTPUT_DIRECTORY_RELEASE  ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_CURRENT_BINARY_DIR}/Binaries

    LIBRARY_OUTPUT_DIRECTORY    ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    LIBRARY_OUTPUT_DIRECTORY_DEBUG    ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    LIBRARY_OUTPUT_DIRECTORY_RELEASE  ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    LIBRARY_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    LIBRARY_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_CURRENT_BINARY_DIR}/Binaries

    ARCHIVE_OUTPUT_DIRECTORY    ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    ARCHIVE_OUTPUT_DIRECTORY_DEBUG    ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    ARCHIVE_OUTPUT_DIRECTORY_RELEASE  ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_CURRENT_BINARY_DIR}/Binaries
    ARCHIVE_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_CURRENT_BINARY_DIR}/Binaries

    OBJECT_OUTPUT_DIRECTORY     ${CMAKE_CURRENT_BINARY_DIR}/Intermediates
    OBJECT_OUTPUT_DIRECTORY_DEBUG    ${CMAKE_CURRENT_BINARY_DIR}/Intermediates
    OBJECT_OUTPUT_DIRECTORY_RELEASE  ${CMAKE_CURRENT_BINARY_DIR}/Intermediates
    OBJECT_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_CURRENT_BINARY_DIR}/Intermediates
    OBJECT_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_CURRENT_BINARY_DIR}/Intermediates
)


# Define the target framework
# set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DOTNET_TARGET_FRAMEWORK_VERSION "v4.7.2")

# Add source files
file(GLOB_RECURSE SOURCES
    "Source/*.cs"
)

target_sources(${PROJECT_NAME} PRIVATE ${SOURCES})

add_dependencies(${PROJECT_NAME} Muniffic-ScriptCore)

# Debug configuration
set(CMAKE_CSharp_FLAGS_DEBUG "/debug:full")
set(CMAKE_CSharp_FLAGS_RELEASE "/optimize")

# Dist configuration
set(CMAKE_CSharp_FLAGS_DIST "/optimize+ /debug:none")

# Filter-based settings for configurations
foreach(configuration ${CMAKE_CONFIGURATION_TYPES})
    if(configuration STREQUAL "Debug")
        target_compile_options(${PROJECT_NAME} PRIVATE /debug:full)
    elseif(configuration STREQUAL "Release")
        target_compile_options(${PROJECT_NAME} PRIVATE /optimize)
    elseif(configuration STREQUAL "Dist")
        target_compile_options(${PROJECT_NAME} PRIVATE /optimize+ /debug:none)
    endif()
endforeach()
