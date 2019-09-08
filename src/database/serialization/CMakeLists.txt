
file(GLOB SOURCES code/*.c)

add_library(ddsserialization SHARED ${SOURCES})

target_include_directories(ddsserialization PRIVATE ../../abstraction/os/include)
target_include_directories(ddsserialization PRIVATE ../database/include)
target_include_directories(ddsserialization PRIVATE ../database/code)
target_include_directories(ddsserialization PRIVATE ../../utilities/include)
target_include_directories(ddsserialization PUBLIC include)
