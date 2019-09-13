CMAKE_MINIMUM_REQUIRED(VERSION 3.4.1)

PROJECT("HELLO_HEADERS")

set(SOURCES 
    src/Hello.cpp 
    src/main.cpp 
)

add_executable(hello_headers ${SOURCES})

target_include_directories( hello_headers
	PRIVATE ${PROJECT_SOURCE_DIR}/include
)
