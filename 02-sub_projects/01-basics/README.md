= Basic Sub-Project
:toc:
:toc-placement!:

toc::[]

# Introduction

This example shows how to setup a CMake project that includes sub-projects. The
top level CMakeLists.txt calls the CMakeLists.txt in the sub directories to
create the following:

  * sublibrary1 - A static library
  * sublibrary2 - A header only library
  * subbinary - An executable

The files included in this example are:

```
$ tree
.
├── CMakeLists.txt
├── subbinary
│   ├── CMakeLists.txt
│   └── main.cpp
├── sublibrary1
│   ├── CMakeLists.txt
│   ├── include
│   │   └── sublib1
│   │       └── sublib1.h
│   └── src
│       └── sublib1.cpp
└── sublibrary2
    ├── CMakeLists.txt
    └── include
        └── sublib2
            └── sublib2.h
```

  * link:CMakeLists.txt[] - Top level CMakeLists.txt
  * link:subbinary/CMakeLists.txt[] - to make the executable
  * link:subbinary/main.cpp[] - source for the executable
  * link:sublibrary1/CMakeLists.txt[] - to make a static library
  * link:sublibrary1/include/sublib1/sublib1.h[]
  * link:sublibrary1/src/sublib2.cpp[]
  * link:sublibrary2/CMakeLists.txt[] - to setup header only library
  * link:sublibrary2/include/sublib2/sublib2.h[]

# Concepts

## Adding a Sub-Directory

A CMakeLists.txt file can include and call sub-directories which include a CMakeLists.txt
files.

[source,cmake]
----
add_subdirectory(sublibrary1)
add_subdirectory(sublibrary2)
add_subdirectory(subbinary)
----

## Referencing Sub-Project Directories

When a project is created using the `project()` command, CMake will automatically
create a number of variables which can be used to reference details about the project.
These variables can then be used by other sub-projects or the main project. For exampe,
to reference the source directory for a different project you can use.

[source,cmake]
----
    ${sublibrary1_SOURCE_DIR}
    ${sublibrary2_SOURCE_DIR}
----

The variables created by CMake include:

[cols=",",options="header",]
|=======================================================================
|Variable |Info
|PROJECT_NAME | The name of the project set by the current `project()`.

|CMAKE_PROJECT_NAME |the name of the first project set by the `project()`
command, i.e. the top level project.

|PROJECT_SOURCE_DIR |The source director of the current project.

|PROJECT_BINARY_DIR |The build directory for the current project.

|name_SOURCE_DIR | The source directory of the project called "name".
In this example the source directories created would be `sublibrary1_SOURCE_DIR`,
`sublibrary2_SOURCE_DIR`, and `subbinary_SOURCE_DIR`

|name_BINARY_DIR | The binary directory of the project called "name".
In this example the binary directories created would be `sublibrary1_BINARY_DIR`,
`sublibrary2_BINARY_DIR`, and `subbinary_BINARY_DIR`

|=======================================================================

## Header only Libraries

If you have a library that is created as a header only library, cmake supports the +INTERFACE+
target to allow creating a target without any build output. More details can be found from
link:https://cmake.org/cmake/help/v3.4/command/add_library.html#interface-libraries[here]

[source,cmake]
----
add_library(${PROJECT_NAME} INTERFACE)
----

When creating the target you can also include directories for that target using
the +INTERFACE+ scope. The +INTERFACE+ scope is use to make target requirements that are used in any Libraries
that link this target but not in the complation of the target itself.

[source,cmake]
----
target_include_directories(${PROJECT_NAME}
    INTERFACE
        ${PROJECT_SOURCE_DIR}/include
)
----

## Referencing Libraries from Sub-Projects

If a sub-project creates a library, it can be referenced by other projects by
calling the name of the project in the `target_link_libraries()` command. This
means that you don't have to reference the full path of the new library and it
is added as a dependency.

[source,cmake]
----
target_link_libraries(subbinary
    PUBLIC
        sublibrary1
)
----

Alternatively, you can create an alias target which allows you to reference the
target in read only contexts.

To create an alias target run:

[source,cmake]
----
add_library(sublibrary2)
add_library(sub::lib2 ALIAS sublibrary2)
----

To reference the alias, just it as follows:
[source,cmake]
----
target_link_libraries(subbinary
    sub::lib2
)
----

## Include directories from sub-projects

When adding the libraries from the sub-projects, starting from cmake v3, there is
no need to add the projects include directores in the include directories of the
binary using them.

This is controlled by the scope in the `target_include_directories()` command when creating
the libraries. In this example because the subbinary executable links the sublibrary1
and sublibrary2 libraries it will automatically include the `${sublibrary1_SOURCE_DIR}/inc`
and `${sublibrary2_SOURCE_DIR}/inc` folders as they are exported with the
 +PUBLIC+ and +INTERFACE+ scopes of the libraries.

# Building the example

[source,bash]
----
$ mkdir build

$ cd build/

$ cmake ..

