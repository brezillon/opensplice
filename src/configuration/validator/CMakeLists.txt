

file(GLOB SOURCES code/*.c)

add_library(ddsconfvalidator SHARED ${SOURCES})

target_include_directories(ddsconfvalidator PUBLIC include)

# target_include_directories(ddsuser PRIVATE ../kernel/include)
# target_include_directories(ddsuser PRIVATE ${CMAKE_BINARY_DIR}/src/kernel)

target_link_libraries(ddsconfvalidator PUBLIC ddsos ddsdatabase ddsutil ddsconf ddsconfparser)

install(TARGETS ddsconfvalidator)
