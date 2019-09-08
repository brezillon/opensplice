

file(GLOB SOURCES code/*.c)

add_library(ddsuser SHARED ${SOURCES})

target_include_directories(ddsuser PUBLIC include)

target_include_directories(ddsuser PRIVATE ../kernel/include)
target_include_directories(ddsuser PRIVATE ${CMAKE_BINARY_DIR}/src/kernel)

target_link_libraries(ddsuser PUBLIC
    ddsos
    ddsdatabase
    ddsconf
    ddsconfparser
    ddsconfvalidator
    ddsserialization
    ddskernel)

install(TARGETS ddsuser)