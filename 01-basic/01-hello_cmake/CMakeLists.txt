# Specify the minimum version of cmake required.
# To find cmake version "cmake --version" on cmd
CMAKE_MINIMUM_REQUIRED(VERSION 3.4.1)

# Specify the name of the project
PROJECT("HELLO_CMAKE")


# Create the executable
ADD_EXECUTABLE(hellocmake main.cpp)
