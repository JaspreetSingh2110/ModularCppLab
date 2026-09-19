# Placeholder umbrella / Facade lib — filled in Phase D
add_library(big_lib INTERFACE)
target_include_directories(big_lib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(big_lib INTERFACE feature_a feature_b)
