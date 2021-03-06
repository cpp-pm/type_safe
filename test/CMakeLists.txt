# Copyright (C) 2016-2019 Jonathan Müller <jonathanmueller.dev@gmail.com>
# This file is subject to the license terms in the LICENSE file
# found in the top-level directory of this distribution.

hunter_add_package(Catch)
find_package(Catch2 CONFIG REQUIRED)

set(source_files test.cpp
                 arithmetic_policy.cpp
                 boolean.cpp
                 bounded_type.cpp
                 compact_optional.cpp
                 constrained_type.cpp
                 constant_parser.cpp
                 deferred_construction.cpp
                 downcast.cpp
                 flag.cpp
                 flag_set.cpp
                 floating_point.cpp
                 index.cpp
                 integer.cpp
                 narrow_cast.cpp
                 optional.cpp
                 optional_ref.cpp
                 output_parameter.cpp
                 reference.cpp
                 strong_typedef.cpp
                 tagged_union.cpp
                 variant.cpp
                 visitor.cpp)
add_executable(type_safe_test debugger_type.hpp ${source_files})
target_link_libraries(type_safe_test PUBLIC type_safe Catch2::Catch2)
target_include_directories(type_safe_test PUBLIC ${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR})
set_property(TARGET type_safe_test PROPERTY CXX_STANDARD 14) # some tests require 14

if(MSVC)
    target_compile_definitions(type_safe_test PRIVATE TYPE_SAFE_TEST_NO_STATIC_ASSERT)
elseif(CMAKE_COMPILER_IS_GNUCC AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 5.0)
    target_compile_definitions(type_safe_test PRIVATE TYPE_SAFE_TEST_NO_STATIC_ASSERT)
endif()

add_test(NAME test COMMAND type_safe_test)
