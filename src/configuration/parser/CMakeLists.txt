

file(GLOB SOURCES code/*.c)

add_library(ddsconfparser SHARED ${SOURCES})

target_include_directories(ddsconfparser PUBLIC include)

# target_include_directories(ddsuser PRIVATE ../kernel/include)
# target_include_directories(ddsuser PRIVATE ${CMAKE_BINARY_DIR}/src/kernel)

target_link_libraries(ddsconfparser PUBLIC ddsconf ddsutil)

install(TARGETS ddsconfparser)
