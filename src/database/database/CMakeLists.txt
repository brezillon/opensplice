
file(GLOB SOURCES code/*.c)

add_library(ddsdatabase SHARED ${SOURCES})

target_include_directories(ddsdatabase PUBLIC include)

target_include_directories(ddsdatabase PRIVATE ../../abstraction/os/include)
target_include_directories(ddsdatabase PRIVATE ../../utilities/include)

target_link_libraries(ddsdatabase PUBLIC m)

install(TARGETS ddsdatabase)