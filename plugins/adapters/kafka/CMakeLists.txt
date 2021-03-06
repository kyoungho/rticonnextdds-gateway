#
# (c) 2020 Copyright, Real-Time Innovations, Inc.  All rights reserved.
#
# RTI grants Licensee a license to use, modify, compile, and create derivative
# works of the Software.  Licensee has the right to distribute object form
# only for use with RTI products.  The Software is provided "as is", with no
# warranty of any type, including any warranty for fitness for any purpose.
# RTI is under no obligation to maintain or support the Software.  RTI shall
# not be liable for any incidental or consequential damages arising out of the
# use or inability to use the software.
# 

###############################################################################
# Set variables
###############################################################################
set(RSPLUGIN_LIB_NAME rtikafkaadapter)

set(KAFKA_LIBRARY "rdkafka")

#set(RDKAFKA_BUILD_STATIC ON CACHE BOOL "")

###############################################################################
# Create library
###############################################################################
set(RTI_KAFKA_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/srcC/KafkaAdapter.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/srcC/KafkaConnection.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/srcC/KafkaStreamReader.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/srcC/KafkaStreamWriter.c"
)

add_library(${RSPLUGIN_LIB_NAME}
    SHARED
        ${RTI_KAFKA_SOURCES}
)

set(RTI_KAFKA_INCLUDES
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/srcC>"
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/rti>"
    "$<BUILD_INTERFACE:${LIBRD_KAFKA_C_DIR}/src>"
    "$<INSTALL_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/rti>"
)

target_include_directories(${RSPLUGIN_LIB_NAME}
    PUBLIC
        ${CONNEXTDDS_INCLUDE_DIRS}
        ${RTI_KAFKA_INCLUDES}
)

target_link_libraries(${RSPLUGIN_LIB_NAME}
    PUBLIC
        ${ROUTING_SERVICE_C_LIBRARIES}
        ${KAFKA_LIBRARY}
)

configure_plugin_defines()

target_compile_definitions(${RSPLUGIN_LIB_NAME}
    PUBLIC
        ${${RSPLUGIN_PREFIX}_DEFINES}
)

# Stagging targets
install(
    TARGETS ${RSPLUGIN_LIB_NAME}
    DESTINATION "${STAGING_LIB_DIR}"
)

install(
    DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/rti/"
    DESTINATION "${STAGING_INCLUDE_DIR}/rti/${STAGING_PLUGIN_DIR}"
)

###############################################################################
# Build tests and documentation if they are enabled
###############################################################################
#if(RTIGATEWAY_ENABLE_DOCS)
#    add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/doc")
#endif()
