CMAKE_MINIMUM_REQUIRED(VERSION 3.4.1)

PROJECT(HELLO_STATIC_LIB)

# Create a library

add_library(hello_library STATIC 
	    src/hello.cpp)

target_include_directories( hello_library
	PUBLIC 
	${PROJECT_SOURCE_DIR}/include
)

add_executable( hello_staticlib 
	        src/main.cpp)


# Link the new hello_library target with the hello_staticlib target
target_link_libraries(hello_staticlib
	PRIVATE
	hello_library	
)
