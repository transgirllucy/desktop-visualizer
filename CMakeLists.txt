cmake_minimum_required(VERSION 4.0)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build (Debug or Release)" FORCE)
endif()

project("Desktop Visualizer")
set(EXECUTABLE_NAME "tblvis")

add_executable(${EXECUTABLE_NAME} main.cpp)

# X11
find_package(X11)
include_directories(${X11_INCLUDE_DIR})
target_link_libraries(${EXECUTABLE_NAME} ${X11_LIBRARIES})

include(FetchContent)
FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 3.0.0
    GIT_SHALLOW ON
    EXCLUDE_FROM_ALL
    SYSTEM)
FetchContent_MakeAvailable(SFML)

# fftw
find_package(FFTW3)
include_directories(${FFTW3_INCLUDE_DIRS})
target_link_libraries(${EXECUTABLE_NAME} ${FFTW3_LIBRARIES})

# PulseAudio
find_package(PulseAudio)
include_directories(${PULSEAUDIO_INCLUDE_DIR})
target_link_libraries(${EXECUTABLE_NAME} ${PULSEAUDIO_LIBRARY})
target_link_libraries(${EXECUTABLE_NAME} pulse-simple)

# pthread
find_package(Threads)
target_link_libraries (${EXECUTABLE_NAME} ${CMAKE_THREAD_LIBS_INIT} SFML::Graphics)

install(TARGETS ${EXECUTABLE_NAME} DESTINATION bin)
