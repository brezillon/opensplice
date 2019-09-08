file(GLOB SOURCES code/*.c)
file(GLOB HEADERS include/*.h)

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

add_library(dcpscommon OBJECT
    ${SOURCES}
    ${HEADERS}
    ${CMAKE_CURRENT_BINARY_DIR}/dds_namedQosTypesSplType.h
    ${CMAKE_CURRENT_BINARY_DIR}/dds_dcps_builtintopicsSplType.h)

set_property(TARGET dcpscommon PROPERTY CMAKE_POSITION_INDEPENDANT_CODE ON)

target_include_directories(dcpscommon PUBLIC include)
target_include_directories(dcpscommon PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(dcpscommon PRIVATE ${CMAKE_BINARY_DIR}/src/kernel)

target_link_libraries(dcpscommon PUBLIC
    ddsdatabase
    ddsos
    ddskernel
    ddsutil
    ddsuser)