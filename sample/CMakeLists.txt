# Phase D sample: small libs → umbrella BigLib → app
# Enabled with -DDP_BUILD_SAMPLE=ON once we start Phase D.

add_subdirectory(libs/core)
add_subdirectory(libs/feature_a)
add_subdirectory(libs/feature_b)
add_subdirectory(libs/big_lib)
add_subdirectory(app)
