cmake_minimum_required(VERSION 3.5.0)
project(MqttSim VERSION 0.1.0 LANGUAGES C CXX)

# Mosquitto 라이브러리 경로
set(MOSQUITTO_DIR ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/mosquitto)
# define CMAKE values
set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui)

set(SOURCE # project source files
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_demo.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

#added include directories
include_directories(
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/glfw/include/
    ${MOSQUITTO_DIR}/include/
)

#set project library path
if(WIN32) # if Windows
    if(CMAKE_CL_64) # if 64bit windows
        set(LIB_DIR ${CMAKE_CURRENT_SOURCE_DIR}/libs/glfw/lib-vc2022)
    else() # if 32bit windows
        set(LIB_DIR ${CMAKE_CURRENT_SOURCE_DIR}/libs/glfw/lib-vc2022)
    endif()
elseif(UNIX) # else if Linux
    add_compile_options(-g -Wall -Wextra)
endif()

add_executable(${PROJECT_NAME}
    ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp
    ${SOURCE}
)

if(WIN32) # if Windows
    target_link_libraries(${PROJECT_NAME}
        PUBLIC
        ${LIB_DIR}/glfw3.lib
        opengl32.lib
        gdi32.lib
        ${MOSQUITTO_DIR}/libraries/mosquitto.lib
        ${MOSQUITTO_DIR}/libraries/mosquittopp.lib
    )
elseif(UNIX) # else if Linux
target_link_libraries(${PROJECT_NAME}
    PUBLIC
    GL
    glfw
    mosquitto
)
endif()