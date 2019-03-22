#
# 1394020320@qq.com
#

cmake_minimum_required(VERSION 3.4.3)
project(souitest)
file (TO_CMAKE_PATH "$ENV{SOUIPATH}" SOUI_ROOT)

message(STATUS "SOUI_ROOT: ${SOUI_ROOT}")
if ("${SOUI_ROOT}" STREQUAL "")
	message(FATAL_ERROR "FATAL:can't find env variable SOUIPATH, clone soui core and install wizard first, please.")
endif()

include(${SOUI_ROOT}/__cmake/cotire.cmake)
include(${SOUI_ROOT}/__cmake/internal_utils.cmake)
include(${SOUI_ROOT}/__cmake/global.cmake)

readSettingFile(UNICODE "1" USE_UNICODE)
readSettingFile(WCHAR "1" WCHAR_AS_DEFAULT)
readSettingFile(MT "0" SHARED_CRT)
readSettingFile(SUPPORT_XP "1" XP_TOOLSET)

#message(STATUS "USE_UNICODE: ${USE_UNICODE}")
#message(STATUS "WCHAR_AS_DEFAULT: ${WCHAR_AS_DEFAULT}")
#message(STATUS "SHARED_CRT: ${SHARED_CRT}")
#message(STATUS "XP_TOOLSET: ${XP_TOOLSET}")

config_compiler_and_linker()

add_definitions(-D_CRT_SECURE_NO_WARNINGS)

include_directories(${SOUI_ROOT}/third-part/gtest/include)
include_directories(${SOUI_ROOT}/components)
include_directories(${SOUI_ROOT}/utilities/include)
include_directories(${SOUI_ROOT}/SOUI/include)
include_directories(${SOUI_ROOT}/config)

file(GLOB_RECURSE CURRENT_HEADERS  *.h *.hpp)
file(GLOB_RECURSE CURRENT_SRCS  *.cpp)

source_group("Header Files" FILES ${CURRENT_HEADERS})
source_group("Source Files" FILES ${CURRENT_SRCS})

link_directories(${SOUI_ROOT}/bin)
add_executable(souitest ${CURRENT_HEADERS} ${CURRENT_SRCS})

#add_dependencies(souitest gtest)
#target_link_libraries(souitest gtest soui utilities ${COM_LIBS})
target_link_libraries(souitest
        debug gtestd optimized gtest
        debug souid optimized soui
        debug utilitiesd optimized utilities
)


set_target_properties(souitest PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})
set_target_properties (souitest PROPERTIES
    FOLDER demos
)
cotire(souitest)
