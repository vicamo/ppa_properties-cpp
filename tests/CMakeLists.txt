if(IS_DIRECTORY /usr/src/googletest/googlemock)
  set(gmock_srcdir /usr/src/googletest/googlemock)
  set(gtest_srcdir /usr/src/googletest/googletest)
elseif(IS_DIRECTORY /usr/src/gmock)
  set(gmock_srcdir /usr/src/gmock)
  set(gtest_srcdir /usr/src/gmock/gtest)
else()
  message(FATAL_ERROR "Can't find gmock. Where is it?")
endif()

include(ExternalProject)
ExternalProject_Add(
  googletest
  SOURCE_DIR ${gmock_srcdir}
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
  ${BINARY_DIR}/libgmock_main.a
  ${BINARY_DIR}/libgmock.a
  ${BINARY_DIR}/gtest/libgtest_main.a
  ${BINARY_DIR}/gtest/libgtest.a
)
# Ubuntu Xenial/Trusty have different source folder layout with
# others. They store google-test source to /usr/src/gtest instead of
# /usr/src/googletest/googletest as others do.
set(
  GTEST_INCLUDE_DIRS
  ${SOURCE_DIR}/include
  ${gtest_srcdir}/include
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
