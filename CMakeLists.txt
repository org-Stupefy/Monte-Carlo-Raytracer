cmake_minimum_required(VERSION 3.10)

set(PROJECT_NAME Monte-Carlo)
project(${PROJECT_NAME} CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

# if(WIN32)
	set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
# endif(WIN32)

# To force the project to use pthreads
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

# GLM
find_package(GLM REQUIRED)
message(STATUS "GLM included at ${GLM_INCLUDE_DIR}")

# find_package(YAMLCPP REQUIRED)
# message(STATUS "Yaml-Cpp included at ${YAMLCPP_INCLUDE_DIR}")

include_directories(
	"${CMAKE_SOURCE_DIR}/thirdparty/glm"
	"${CMAKE_SOURCE_DIR}/thirdparty/yaml-cpp"
)



file(GLOB_RECURSE _source_list ${PROJECT_SOURCE_DIR}/source/*.cpp ${PROJECT_SOURCE_DIR}/source/*.h ${PROJECT_SOURCE_DIR}/scenes/*.yml)

foreach(_source IN ITEMS ${_source_list})
    get_filename_component(_source_path "${_source}" PATH)
    string(REPLACE "${CMAKE_SOURCE_DIR}" "" _group_path "${_source_path}")
    string(REPLACE "/" "\\" _group_path "${_group_path}")
    source_group("${_group_path}" FILES "${_source}")
endforeach()

add_executable(${PROJECT_NAME} ${_source_list})
target_link_libraries(${PROJECT_NAME} Threads::Threads)