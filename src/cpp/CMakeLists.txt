file(GLOB SOURCES code/*.c)

add_library(ddscpp SHARED ${SOURCES})

target_include_directories(ddscpp PUBLIC include)

target_link_libraries(ddscpp PUBLIC ddsos)