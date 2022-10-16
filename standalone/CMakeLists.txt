cmake_minimum_required(VERSION 3.14...3.22)

project(GreeterStandalone LANGUAGES CXX)

# --- Import tools ----

include(../cmake/tools.cmake)

# ---- Dependencies ----

include(../cmake/CPM.cmake)

CPMAddPackage(
  GITHUB_REPOSITORY jarro2783/cxxopts
  VERSION 3.0.0
  OPTIONS "CXXOPTS_BUILD_EXAMPLES NO" "CXXOPTS_BUILD_TESTS NO" "CXXOPTS_ENABLE_INSTALL YES"
)

CPMAddPackage(NAME Greeter SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/..)

find_package(PkgConfig) 
pkg_search_module(GLIB REQUIRED glib-2.0) 
pkg_check_modules(GSTREAMER REQUIRED gstreamer-1.0)
pkg_check_modules(GST_APP REQUIRED gstreamer-app-1.0)
pkg_check_modules(GST_VIDEO REQUIRED gstreamer-video-1.0)
pkg_check_modules(GSTREAMER-PBUTILS REQUIRED gstreamer-pbutils-1.0)

include_directories(
  ${GLIB_INCLUDE_DIRS}
  ${GSTREAMER_INCLUDE_DIRS}
  ${GST_APP_INCLUDE_DIRS}
  ${GSTREAMER-AUDIO_INCLUDE_DIRS}
  ${GSTREAMER-PBUTILS_INCLUDE_DIRS}
  ${GSTREAMER-FFT_INCLUDE_DIRS}
  ${CMAKE_CURRENT_SOURCE_DIR}
)

# ---- Create standalone executable ----

file(GLOB sources CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/source/*.cpp)

# link_directories(${GSTLIBS_LIBRARY_DIRS})
link_directories(
  ${GLIB_LIBRARY_DIRS}
  ${GSTREAMER_LIBRARY_DIRS}
  ${GST_APP_LIBRARY_DIRS}
  ${GSTREAMER-AUDIO_LIBRARY_DIRS}
  ${GSTREAMER-PBUTILS_LIBRARY_DIRS}
  ${GSTREAMER-FFT_LIBRARY_DIRS}
)
add_executable(${PROJECT_NAME} ${sources})

set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD 17 OUTPUT_NAME "Greeter")



set(inputtest_LIBRARIES
  ${GSTREAMER_LIBRARIES}
  ${GST_APP_LIBRARIES}
  ${GSTREAMER-AUDIO_LIBRARIES}
  ${GSTREAMER-PBUTILS_LIBRARIES}

  ${GLIB_LIBRARIES}
)

message(STATUS "wat is this: ${inputtest_LIBRARIES}")
target_link_libraries(${PROJECT_NAME} ${inputtest_LIBRARIES} cxxopts)
