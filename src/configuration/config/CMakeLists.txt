

file(GLOB SOURCES code/*.c)

add_library(ddsconf SHARED ${SOURCES})

target_include_directories(ddsconf PUBLIC include)

target_link_libraries(ddsconf PUBLIC ddsos ddsdatabase)

install(TARGETS ddsconf)