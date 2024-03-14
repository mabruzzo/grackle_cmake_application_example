cmake_minimum_required(VERSION 3.16)
project(GrackleExample LANGUAGES C)

if(NOT BUILD_SHARED_LIBS) # in this case, we are using static libraries
  # this statement is currently required when
  #   1. we are "linking" our downstream application against grackle libraries
  #      that are statically compiled
  #   2. AND we don't list Fortran as one of the languages used by our
  #      downstream application (we would list this in the call to the
  #      `project` function)
  # If the above 2 conditions are met and we don't include the following
  # statement there's an issue with linking...
  #
  # NOTE: In the future, we can definitely take some steps to solve this within
  # Grackle's CMakeLists.txt files.
  enable_language(Fortran)
endif()

# this line introduces the library build target called Grackle::Grackle
add_subdirectory(external/grackle)

# this creates the build target, example_app, which is our output executable
# program and lists all source files needed to build it
add_executable(example_app src/c_example.c)

# this line informs cmake to "link" example_app executable against the
# Grackle::Grackle target (this conveys information about header locations and
# gracefully handles the cases where grackle is compiled as either a shared or
# a static library)
target_link_libraries(example_app PRIVATE Grackle::Grackle)

