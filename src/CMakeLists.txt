cmake_minimum_required(VERSION 2.8)
find_package(ZLIB REQUIRED)
find_package(Qt4 REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Boost COMPONENTS thread REQUIRED)
find_package(Boost COMPONENTS filesystem REQUIRED)
find_package(Boost COMPONENTS system REQUIRED)
find_package(Boost COMPONENTS date_time REQUIRED)

include(${QT_USE_FILE})
 
qt4_wrap_cpp(main_moc_SRCS
             main.h)

IF(UNIX)
    set(CMAKE_CXX_FLAGS "-O3 -msse2 -msse3")
ENDIF (UNIX)

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/Include)
INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${ZLIB_INCLUDE_DIR})
INCLUDE_DIRECTORIES("/usr/include/qt4/QtTest/")

set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)
set(BOOST_ALL_DYN_LINK ON)   # force dynamic linking for all libraries

set(CMAKE_CXX_FLAGS "-O3 -msse2 -msse3")

if(APPLE)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libOpenNI2.dylib ${CMAKE_CURRENT_BINARY_DIR}/libOpenNI2.dylib COPYONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libOniFile.dylib ${CMAKE_CURRENT_BINARY_DIR}/OpenNI2/Drivers/libOniFile.dylib COPYONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libPS1080.dylib ${CMAKE_CURRENT_BINARY_DIR}/OpenNI2/Drivers/libPS1080.dylib COPYONLY)
elseif(UNIX)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libOpenNI2.so ${CMAKE_CURRENT_BINARY_DIR}/libOpenNI2.so COPYONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libDummyDevice.so ${CMAKE_CURRENT_BINARY_DIR}/OpenNI2/Drivers/libDummyDevice.so COPYONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libOniFile.so ${CMAKE_CURRENT_BINARY_DIR}/OpenNI2/Drivers/libOniFile.so COPYONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/libPS1080.so ${CMAKE_CURRENT_BINARY_DIR}/OpenNI2/Drivers/libPS1080.so COPYONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/librs2driver.so ${CMAKE_CURRENT_BINARY_DIR}/OpenNI2/Drivers/librs2driver.so COPYONLY)
endif()

set(CMAKE_EXE_LINKER_FLAGS "-Wl,-rpath,. -L${CMAKE_CURRENT_BINARY_DIR}")

set(srcs main.cpp
         Logger2.cpp
         Communicator.cpp
         ${CMAKE_CURRENT_SOURCE_DIR}/OpenNI2/OpenNI2Interface.cpp)

add_executable(Logger2 ${main_moc_SRCS} ${srcs})

target_link_libraries(Logger2                       
                      ${ZLIB_LIBRARY}
                      ${Boost_LIBRARIES}
                      ${OpenCV_LIBS} 
                      ${QT_LIBRARIES}
                      QtTest
                      boost_system
                      boost_filesystem
                      boost_thread
                      OpenNI2)

