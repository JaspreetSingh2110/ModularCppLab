# Placeholder — filled in Phase D
add_library(feature_b INTERFACE)
target_include_directories(feature_b INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(feature_b INTERFACE core)
