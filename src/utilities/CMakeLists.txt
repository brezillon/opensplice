file(GLOB SOURCES code/*.c)

add_library(ddsutil SHARED ${SOURCES})
target_include_directories(ddsutil PRIVATE code)
target_include_directories(ddsutil PUBLIC include)
target_include_directories(ddsutil PUBLIC ../abstraction/os/include)
target_include_directories(ddsutil PUBLIC ../database/database/include)
target_include_directories(ddsutil PUBLIC ../../external/snappy-c/src/)

install(TARGETS ddsutil)