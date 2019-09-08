

file(GLOB SOURCES code/*.c)

add_library(ddsos SHARED
    ${SOURCES}
    linux/code/os__process.c
    linux/code/os__library.c
    linux/code/os__linuxEntropyHook.c
    linux/code/os__cfg_weak.c)

target_include_directories(ddsos PRIVATE linux linux/include ../os-net/include)
target_include_directories(ddsos PUBLIC include)

target_link_libraries(ddsos PUBLIC pthread rt dl)

install(TARGETS ddsos)