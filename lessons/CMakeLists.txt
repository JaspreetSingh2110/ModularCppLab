# Adds a lesson target only when that folder has its own CMakeLists.txt
function(dp_add_lesson relative_dir)
  set(_lesson_cmake "${CMAKE_CURRENT_SOURCE_DIR}/${relative_dir}/CMakeLists.txt")
  if(EXISTS "${_lesson_cmake}")
    add_subdirectory(${relative_dir})
  endif()
endfunction()

# Phase A — Modern C++
dp_add_lesson(phase_a/a1_cpp14)
dp_add_lesson(phase_a/a2_cpp17)
dp_add_lesson(phase_a/a3_ownership)
dp_add_lesson(phase_a/a4_cpp20)
dp_add_lesson(phase_a/a5_cpp23_26)
dp_add_lesson(phase_a/a6_cmake_tooling)

# Phase B — Design patterns
dp_add_lesson(phase_b/b1_foundations)
dp_add_lesson(phase_b/b2_dependency_inversion)
dp_add_lesson(phase_b/b2_facade)
dp_add_lesson(phase_b/b2_adapter)
dp_add_lesson(phase_b/b2_strategy)
dp_add_lesson(phase_b/b2_factory)
dp_add_lesson(phase_b/b2_observer)
dp_add_lesson(phase_b/b2_pimpl)
dp_add_lesson(phase_b/b3_decorator)
dp_add_lesson(phase_b/b3_command)
dp_add_lesson(phase_b/b3_builder)
dp_add_lesson(phase_b/b3_state)
dp_add_lesson(phase_b/b3_singleton)
dp_add_lesson(phase_b/b4_synthesis)

# Phase C — Integration
dp_add_lesson(phase_c/c1_strategy_factory)
dp_add_lesson(phase_c/c2_adapter_facade)
dp_add_lesson(phase_c/c3_pimpl_headers)
dp_add_lesson(phase_c/c4_antipatterns)
