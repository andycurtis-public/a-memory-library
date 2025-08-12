# SPDX-FileCopyrightText: 2023-2025 Andy Curtis <contactandyc@gmail.com>
# SPDX-License-Identifier: Apache-2.0
cmake_minimum_required(VERSION 3.10)
project(a-json-tests VERSION 1.0)

# Enable testing
enable_testing()

# Define the library to test
set(LIB_TO_TEST a-memory-library)

# Set the directory for test sources
set(TEST_SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/src/test_aml_alloc.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/test_aml_buffer.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/test_aml_pool.c
)

set(CUSTOM_PACKAGES the-macro-library)

find_package(a-cmake-library REQUIRED)

include(BinaryConfig)
