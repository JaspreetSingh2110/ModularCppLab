# Placeholder — filled in Phase D
add_library(feature_a INTERFACE)
target_include_directories(feature_a INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(feature_a INTERFACE core)
