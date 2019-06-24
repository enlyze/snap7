cmake_minimum_required(VERSION 3.10)
project(snap7 CXX)

list(APPEND SOURCES
    src/core/s7_client.cpp
    src/core/s7_isotcp.cpp
    src/core/s7_micro_client.cpp
    src/core/s7_partner.cpp
    src/core/s7_peer.cpp
    src/core/s7_server.cpp
    src/core/s7_text.cpp
    src/lib/snap7_libmain.cpp
    src/sys/snap_msgsock.cpp
    src/sys/snap_sysutils.cpp
    src/sys/snap_tcpsrvr.cpp
    src/sys/snap_threads.cpp)

if(WIN32)
    list(APPEND SOURCES src/lib/snap7.def)
endif()

add_library(snap7 SHARED ${SOURCES})
set_property(TARGET snap7 PROPERTY POSITION_INDEPENDENT_CODE ON)
target_include_directories(snap7 PRIVATE
    src/core
    src/lib
    src/sys)
set_target_properties(snap7 PROPERTIES
	VERSION 1.4.2
	SOVERSION 1)

if(WIN32)
    target_compile_definitions(snap7 PRIVATE
        _CRT_NONSTDC_NO_DEPRECATE
        _CRT_SECURE_NO_WARNINGS
        _WINSOCK_DEPRECATED_NO_WARNINGS)
    target_link_libraries(snap7 PRIVATE
        winmm
        ws2_32)
else()
    include(GNUInstallDirs)
    install(TARGETS snap7 LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
endif()
