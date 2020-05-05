#
#
#
macro(package_add_test TESTNAME_)
    set(TEST_NAME "modules.${TESTNAME_}")
    
    # create an exectuable in which the tests will be stored
    add_executable(${TEST_NAME} ${ARGN})
    # link the Google test infrastructure, mocking library, and a default main fuction to
    # the test executable.  Remove g_test_main if writing your own main function.
    target_link_libraries(${TEST_NAME} gtest gmock gtest_main)
    # gtest_discover_tests replaces gtest_add_tests,
    # see https://cmake.org/cmake/help/v3.10/module/GoogleTest.html for more options to pass to it
    gtest_discover_tests(${TEST_NAME}
        # set a working directory so your project root so that you can find test data via paths relative to the project root
        WORKING_DIRECTORY ${PROJECT_DIR}
        PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY "${PROJECT_DIR}"
    )
    set_target_properties(${TEST_NAME} PROPERTIES FOLDER tests)
endmacro()

package_add_test(utility.portcheck utility/portcheck.cpp)