
file(GLOB_RECURSE SOURCES code/*.cpp)
file(GLOB_RECURSE COMMON ../common/code/*.c)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/dds_namedQosTypesSplType.h
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/dds_namedQosTypesSplLoad.c
    # OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplT.c
    COMMAND idlpp -l c -S -m SPLLOAD -m SPLTYPE -d ${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/etc/idl/dds_namedQosTypes.idl
    MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/etc/idl/dds_namedQosTypes.idl
    DEPENDS idlpp)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/dds_builtinTopicsSplType.h
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/dds_builtinTopicsSplLoad.c
    # OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplT.c
    COMMAND idlpp -I${CMAKE_SOURCE_DIR}/etc/idl -l c -S -m SPLLOAD -m SPLTYPE -d ${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/etc/idl/dds_builtinTopics.idl
    MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/etc/idl/dds_builtinTopics.idl
    DEPENDS idlpp)

add_library(dcpsisocpp2 SHARED ${SOURCES} ${COMMON} ${CMAKE_CURRENT_BINARY_DIR}/dds_builtinTopicsSplType.h ${CMAKE_CURRENT_BINARY_DIR}/dds_namedQosTypesSplType.h)# $<TARGET_OBJECTS:dcpscommon>)

target_include_directories(dcpsisocpp2
    PUBLIC
        $<INSTALL_INTERFACE:include/dcps/C++/isocpp2>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>)
target_include_directories(dcpsisocpp2 PRIVATE ${CMAKE_BINARY_DIR}/src/kernel)

target_link_libraries(dcpsisocpp2 PUBLIC ddsos ddsuser ddskernel)

target_include_directories(dcpsisocpp2 PRIVATE ../common/include)
target_include_directories(dcpsisocpp2 PRIVATE ${CMAKE_CURRENT_BINARY_DIR})

install(TARGETS dcpsisocpp2)

install(DIRECTORY include/ DESTINATION include/dcps/C++/isocpp2)