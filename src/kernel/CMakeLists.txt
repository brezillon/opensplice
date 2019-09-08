

file(GLOB SOURCES code/*.c)

if (WIN32)
    set(PREPROCESSOR "${CMAKE_C_COMPILER} /P")
else()
    set(PREPROCESSOR cpp)
endif()

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/v_kernel_odl.pp
    COMMAND ${PREPROCESSOR} -I${CMAKE_SOURCE_DIR}/src/abstraction/os/include ${CMAKE_CURRENT_SOURCE_DIR}/code/v_kernel.odl ${CMAKE_CURRENT_BINARY_DIR}/v_kernel_odl.pp
    MAIN_DEPENDENCY code/v_kernel.odl)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/kernelModuleI.h
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/kernelModule.h
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/kernelModuleI.c
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/kernelModule.c
    COMMAND odlpp ${CMAKE_BINARY_DIR}/v_kernel.odl.pp
    MAIN_DEPENDENCY ${CMAKE_CURRENT_BINARY_DIR}/v_kernel_odl.pp
    DEPENDS odlpp)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/dds_builtinTopicsSplType.h
    # OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplT.c
    COMMAND idlpp -I${CMAKE_SOURCE_DIR}/etc/idl -l c -S -m SPLLOAD -m SPLTYPE -d ${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/etc/idl/dds_builtinTopics.idl
    MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/etc/idl/dds_builtinTopics.idl
    DEPENDS idlpp)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/dds_dcps_builtintopicsSplType.h
    # OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplT.c
    COMMAND idlpp -I${CMAKE_SOURCE_DIR}/etc/idl -l c -S -m SPLLOAD -m SPLTYPE -d ${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/etc/idl/dds_dcps_builtintopics.idl
    MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/etc/idl/dds_dcps_builtintopics.idl
    DEPENDS idlpp)
    
add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplType.h
    # OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplT.c
    COMMAND idlpp -l c -S -m SPLLOAD -m SPLTYPE -d ${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/etc/idl/client_durability.idl
    MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/etc/idl/client_durability.idl
    DEPENDS idlpp)

add_library(ddskernel SHARED
    ${SOURCES}
    ${CMAKE_CURRENT_BINARY_DIR}/kernelModuleI.c
    ${CMAKE_CURRENT_BINARY_DIR}/kernelModule.c
    ${CMAKE_CURRENT_BINARY_DIR}/client_durabilitySplType.h
    ${CMAKE_CURRENT_BINARY_DIR}/dds_builtinTopicsSplType.h
    ${CMAKE_CURRENT_BINARY_DIR}/dds_dcps_builtintopicsSplType.h
    )

target_include_directories(ddskernel PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(ddskernel PRIVATE ../abstraction/os/include)
target_include_directories(ddskernel PRIVATE ../database/database/include)
target_include_directories(ddskernel PRIVATE ../database/serialization/include)
target_include_directories(ddskernel PRIVATE ../utilities/include)


target_include_directories(ddskernel PUBLIC include)

# add_dependencies(ddskernel odlpp)

install(TARGETS ddskernel)