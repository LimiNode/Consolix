cmake_minimum_required(VERSION 3.14)

project(consolix_parent_event_hub LANGUAGES CXX)

add_library(event_hub_cpp INTERFACE)
add_library(event_hub::event_hub ALIAS event_hub_cpp)

set(CONSOLIX_CXX_STANDARD 17 CACHE STRING "" FORCE)
set(CONSOLIX_USE_LOGIT OFF CACHE BOOL "" FORCE)
set(CONSOLIX_USE_JSON OFF CACHE BOOL "" FORCE)
set(CONSOLIX_USE_CXXOPTS OFF CACHE BOOL "" FORCE)
set(CONSOLIX_USE_EVENT_HUB ON CACHE BOOL "" FORCE)
set(CONSOLIX_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(CONSOLIX_BUILD_TESTS OFF CACHE BOOL "" FORCE)
add_subdirectory(
    "${CMAKE_CURRENT_LIST_DIR}/../../.."
    "${CMAKE_CURRENT_BINARY_DIR}/consolix"
)

get_target_property(consolix_link_libraries Consolix INTERFACE_LINK_LIBRARIES)
if(NOT "event_hub::event_hub" IN_LIST consolix_link_libraries)
    message(FATAL_ERROR "Consolix must link the parent-provided event hub target")
endif()

add_custom_target(parent_event_hub_target_smoke ALL)

enable_testing()
add_test(
    NAME parent_event_hub_target_smoke
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target parent_event_hub_target_smoke
)
