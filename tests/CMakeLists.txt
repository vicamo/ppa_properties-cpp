include(ExternalProject)
ExternalProject_Add(
  googletest
  SOURCE_DIR /usr/src/googletest
  CMAKE_ARGS -DBUILD_GTEST=ON -DBUILD_GMOCK=ON
  INSTALL_COMMAND ""
)
ExternalProject_Get_Property(
  googletest
  SOURCE_DIR
)
ExternalProject_Get_Property(
  googletest
  BINARY_DIR
)
set(
  GTEST_BOTH_LIBRARIES
  ${BINARY_DIR}/googlemock/libgmock_main.a
  ${BINARY_DIR}/googlemock/libgmock.a
  ${BINARY_DIR}/googlemock/gtest/libgtest_main.a
  ${BINARY_DIR}/googlemock/gtest/libgtest.a
)
set(
  GTEST_INCLUDE_DIRS
  ${SOURCE_DIR}/googlemock/include
  ${SOURCE_DIR}/googletest/include
)

include_directories(${GTEST_INCLUDE_DIRS})

add_executable(
  properties_test
  properties_test.cpp
)

add_executable(
  signals_test
  signals_test.cpp
)

add_dependencies(
  properties_test
  googletest
)

add_dependencies(
  signals_test
  googletest
)

target_link_libraries(
  properties_test

  ${GTEST_BOTH_LIBRARIES}
)

target_link_libraries(
  signals_test

  ${GTEST_BOTH_LIBRARIES}
)

add_test(properties_test ${CMAKE_CURRENT_BINARY_DIR}/properties_test)
add_test(signals_test ${CMAKE_CURRENT_BINARY_DIR}/signals_test)
