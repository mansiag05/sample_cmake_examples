CMAKE_MINIMUM_REQUIRED(VERSION 3.4.1)

PROJECT(sharedlib)

add_library(sharedlib SHARED 
	     src/Hello.cpp)

target_include_directories(sharedlib PUBLIC ${PROJECT_SOURCE_DIR}/include)


add_executable(sharedbin src/main.cpp)

target_link_libraries(sharedbin PRIVATE sharedlib)
