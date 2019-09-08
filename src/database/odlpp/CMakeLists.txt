

find_package(FLEX REQUIRED)
find_package(BISON REQUIRED)

flex_target(lexer code/c_odlbase.l ${CMAKE_CURRENT_BINARY_DIR}/c_odlbase.h
    COMPILE_FLAGS "-Pc_odlbase_yy")

bison_target(parser code/c_odlbase.y ${CMAKE_CURRENT_BINARY_DIR}/c_odlbase.c)
set(BISON_TARGET_cmdopt "")

file(GLOB SOURCES code/*.c)

add_executable(odlpp ${SOURCES} ${BISON_parser_OUTPUTS} ${FLEX_lexer_OUTPUTS})

target_include_directories(odlpp PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(odlpp PRIVATE code)

target_link_libraries(odlpp PRIVATE ddsos ddsdatabase ddsutil)