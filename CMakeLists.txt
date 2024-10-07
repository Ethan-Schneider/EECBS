cmake_minimum_required(VERSION 2.6)

project(eecbs)
set(CMAKE_CXX_STANDARD 20)
set(PYBIND11_PYTHON_VERSION 3.12)
set(CMAKE_CXX_FLAGS "-Wall -Wextra -fPIC")

find_package(pybind11 REQUIRED)
find_package(Eigen3 REQUIRED)

include_directories("inc")
include_directories("src")
file(GLOB SOURCES "src/*.cpp")

ADD_LIBRARY(LibsModule
    inc/CBS.h
    inc/CBSHeuristic.h
    inc/CBSNode.h
    inc/common.h
    inc/Conflict.h
    inc/ConstraintPropagation.h
    inc/ConstraintTable.h
    inc/CorridorReasoning.h
    inc/ECBS.h
    inc/ECBSNode.h
    inc/IncrementalPairwiseMutexPropagation.hpp
    inc/Instance.h
    inc/MDD.h
    inc/MutexReasoning.h
    inc/RectangleReasoning.h
    inc/ReservationTable.h
    inc/SingleAgentSolver.h
    inc/SIPP.h
    inc/SpaceTimeAStar.h

    src/CBS.cpp
    src/CBSHeuristic.cpp
    src/CBSNode.cpp
    src/common.cpp
    src/Conflict.cpp
    src/ConstraintPropagation.cpp
    src/ConstraintTable.cpp
    src/CorridorReasoning.cpp
    src/ECBS.cpp
    src/IncrementalPairwiseMutexPropagation.cpp
    src/Instance.cpp
    src/MDD.cpp
    src/MutexReasoning.cpp
    src/RectangleReasoning.cpp
    src/ReservationTable.cpp
    src/SingleAgentSolver.cpp
    src/SIPP.cpp
    src/SpaceTimeAStar.cpp
)

pybind11_add_module(${PROJECT_NAME} src/pywrap.cpp)
target_link_libraries(${PROJECT_NAME} LibsModule)

target_compile_definitions(${PROJECT_NAME} PRIVATE VERSION_INFO=${EXAMPLE_VERSION_INFO})
target_include_directories(${PROJECT_NAME} PRIVATE ${PYBIND11_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} PRIVATE Eigen3::Eigen)

# Find Boost
find_package(Boost REQUIRED COMPONENTS program_options system filesystem)
include_directories( ${Boost_INCLUDE_DIRS} )
target_link_libraries(eecbs ${Boost_LIBRARIES})