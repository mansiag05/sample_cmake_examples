CMAKE_MINIMUM_REQUIRED(VERSION 3.4.1)

PROJECT(Installing)

add_library(sharedlib
	    SHARED
	    src/Hello.cpp	
)

target_include_directories(sharedlib
			   PUBLIC
			   ${PROJECT_SOURCE_DIR}/include
)

add_executable(sharedbin src/main.cpp)

target_link_libraries(sharedbin sharedlib)


############ make install #####################

# Binaries
install (TARGETS sharedbin DESTINATION bin)

# Library
install (TARGETS sharedlib LIBRARY DESTINATION lib)

# Header Files
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)

# config
install (FILES cmake-examples.conf
    DESTINATION etc)



