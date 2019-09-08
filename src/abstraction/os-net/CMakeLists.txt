file(GLOB SOURCES code/*.c)

add_library(ddsosnet SHARED ${SOURCES})

target_include_directories(ddsosnet PRIVATE linux)
target_include_directories(ddsosnet PUBLIC ../os/include)
target_include_directories(ddsosnet PUBLIC include)