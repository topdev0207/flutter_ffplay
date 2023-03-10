cmake_minimum_required(VERSION 3.15)
set(PROJECT_NAME "ffmpeg")
project(${PROJECT_NAME} LANGUAGES CXX)

# This value is used when generating builds using this plugin, so it must
# not be changed
set(PLUGIN_NAME "flutter_ffplay_plugin")

add_library(${PLUGIN_NAME} SHARED
  "${CMAKE_CURRENT_SOURCE_DIR}/../cxx/ffi.cpp"
  "flutter_ffplay_plugin.cpp"
)

include("${CMAKE_CURRENT_SOURCE_DIR}/../cxx/ffmpeg.cmake")
target_include_directories(${PLUGIN_NAME} PRIVATE "${FFMPEG_PATH}/include")

apply_standard_settings(${PLUGIN_NAME})
target_compile_options(${PLUGIN_NAME} PRIVATE /WX-)

set_target_properties(${PLUGIN_NAME} PROPERTIES
  CXX_VISIBILITY_PRESET hidden)
target_compile_definitions(${PLUGIN_NAME} PRIVATE FLUTTER_PLUGIN_IMPL)
target_include_directories(${PLUGIN_NAME} INTERFACE
  "${CMAKE_CURRENT_SOURCE_DIR}/include")
target_link_libraries(${PLUGIN_NAME} PRIVATE flutter flutter_wrapper_plugin
  ${ffmpeg-lib})

# List of absolute paths to libraries that should be bundled with the plugin
set(ffmpeg_bundled_libraries
  ""
  PARENT_SCOPE
)
