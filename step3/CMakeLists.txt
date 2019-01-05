cmake_minimum_required (VERSION 2.6)
project (tutorial)


# The version number.
set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)


# should we use our own math functions?
option (USE_MYMATH "Use tutorial provided math implementation" ON) 


# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
  "${PROJECT_SOURCE_DIR}/tutorialconfig.h.in"
  "${PROJECT_BINARY_DIR}/tutorialconfig.h"
  )


# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
include_directories("${PROJECT_BINARY_DIR}")


# add the MathFunctions library?
if (USE_MYMATH)
  include_directories ("${PROJECT_SOURCE_DIR}/mathfunctions")
  add_subdirectory (mathfunctions)
  set (EXTRA_LIBS ${EXTRA_LIBS} mymath)
endif (USE_MYMATH)


# add the executable
add_executable (tutorial tutorial.cxx)
target_link_libraries (tutorial mymath)


# add the install targets
install (TARGETS tutorial DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/tutorialconfig.h" DESTINATION include)



# enable testing
enable_testing ()

# does the application run
add_test (TutorialRuns tutorial 25)

# does it sqrt of 25
add_test (TutorialComp25 tutorial 25)
set_tests_properties (TutorialComp25 PROPERTIES PASS_REGULAR_EXPRESSION "25 is 5")

# does it handle negative numbers
add_test (TutorialNegative tutorial -25)
set_tests_properties (TutorialNegative PROPERTIES PASS_REGULAR_EXPRESSION "-25 is 0")

# does it handle small numbers
add_test (TutorialSmall tutorial 0.0001)
set_tests_properties (TutorialSmall PROPERTIES PASS_REGULAR_EXPRESSION "0.0001 is 0.01")

# does the usage message work?
add_test (TutorialUsage tutorial)
set_tests_properties (TutorialUsage PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number")
 
