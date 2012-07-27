SET(PROJECT_NAME Sophus)

PROJECT(${PROJECT_NAME})
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


SET (CMAKE_VERBOSE_MAKEFILE ON)

IF( NOT CMAKE_BUILD_TYPE )
   SET( CMAKE_BUILD_TYPE Release )
ENDIF()

IF (CMAKE_COMPILER_IS_GNUCXX )
   SET(CMAKE_CXX_FLAGS_DEBUG  "-O0 -g")
   SET(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG ")

  ADD_DEFINITIONS("-Wall -Werror -Wno-unused-variable
                   -Wno-unused-but-set-variable -Wno-unknown-pragmas ")
ENDIF()

FIND_PACKAGE(Eigen REQUIRED)
INCLUDE_DIRECTORIES(${EIGEN_INCLUDE_DIRS})
ADD_DEFINITIONS(${EIGEN_DEFINITIONS})

SET (SOURCE_DIR "sophus")
SET (CLASSES  so2
              se2
              se3
              so3
              scso3
              sim3
)

SET (SOURCES)

FOREACH(class ${CLASSES})
  LIST(APPEND SOURCES ${SOURCE_DIR}/${class}.cpp)
ENDFOREACH(class)

LINK_LIBRARIES (${PROJECT_NAME} ${LIBS})

ADD_LIBRARY(${PROJECT_NAME} SHARED ${SOURCES})

ADD_EXECUTABLE(test_so2 sophus/test_so2.cpp)
ADD_EXECUTABLE(test_se2 sophus/test_se2.cpp)
ADD_EXECUTABLE(test_so3 sophus/test_so3.cpp)
ADD_EXECUTABLE(test_scso3 sophus/test_scso3.cpp)
ADD_EXECUTABLE(test_se3 sophus/test_se3.cpp)
ADD_EXECUTABLE(test_sim3 sophus/test_sim3.cpp)
ENABLE_TESTING()

ADD_TEST(test_so2 test_so2)
ADD_TEST(test_se2 test_se2)
ADD_TEST(test_so3 test_so3)
ADD_TEST(test_scso3 test_scso3)
ADD_TEST(test_se3 test_se3)
ADD_TEST(test_sim3 test_sim3)



