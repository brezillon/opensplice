file(GLOB SOURCES code/*.c)

find_package(BISON REQUIRED)
find_package(FLEX REQUIRED)

flex_target(lexer code/idl_base.l ${CMAKE_CURRENT_BINARY_DIR}/idl_base.h
    COMPILE_FLAGS "-Pidl_base_yy")

bison_target(parser code/idl_base.y ${CMAKE_CURRENT_BINARY_DIR}/idl_base.c)
set(BISON_TARGET_cmdopt "")

add_executable(idlpp ${SOURCES} ${BISON_parser_OUTPUTS} ${FLEX_lexer_OUTPUTS})

target_include_directories(idlpp PRIVATE code)

target_include_directories(idlpp PUBLIC include)

target_link_libraries(idlpp PRIVATE
    ddsos
    ddsdatabase
    ddsutil
    ddsserialization
    ddscpp)

install(TARGETS idlpp)