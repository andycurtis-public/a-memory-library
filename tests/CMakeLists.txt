cmake_minimum_required(VERSION 3.20)
project(the_macro_library_tests LANGUAGES C)

enable_testing()

# Header-only package
find_package(a_memory_library CONFIG REQUIRED)
find_package(the_macro_library CONFIG REQUIRED)

# Optional system libs (platform-dependent)
find_library(M_LIB  m)
find_library(RT_LIB rt)

# Small helper to define & register each test
function(make_test name src)
  add_executable(${name} "${src}")
  set_target_properties(${name} PROPERTIES
    C_STANDARD 99
    C_STANDARD_REQUIRED YES
  )
  target_link_libraries(${name} PRIVATE a_memory_library::a_memory_library the_macro_library::the_macro_library)
  if(M_LIB)
    target_link_libraries(${name} PRIVATE ${M_LIB})
  endif()
  if(RT_LIB)
    target_link_libraries(${name} PRIVATE ${RT_LIB})
  endif()

  # Warnings (same style you used in a-json-library/tests)
  if(MSVC)
    target_compile_options(${name} PRIVATE /W4)
  else()
    target_compile_options(${name} PRIVATE -Wall -Wextra -Wpedantic)
  endif()

  add_test(NAME ${name} COMMAND ${name})
endfunction()

# Tests (one binary per source, as your build.sh runs them individually)
make_test(test_aml_alloc "${CMAKE_CURRENT_SOURCE_DIR}/src/test_aml_alloc.c")
make_test(test_aml_buffer "${CMAKE_CURRENT_SOURCE_DIR}/src/test_aml_buffer.c")
make_test(test_aml_pool "${CMAKE_CURRENT_SOURCE_DIR}/src/test_aml_pool.c")
