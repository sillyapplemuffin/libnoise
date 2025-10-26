set(PROJECT_NAME noiseutils)
set(LIB_NAME noiseutils)

add_library(${LIB_NAME} noiseutils.cpp)
add_library(noiseutils::${LIB_NAME} ALIAS ${LIB_NAME})

target_link_libraries(${LIB_NAME} PUBLIC noise::noise)

set(structured_include_dir ${CMAKE_CURRENT_BINARY_DIR}/include)
file(MAKE_DIRECTORY ${structured_include_dir}/noiseutils)
file(COPY noiseutils.h DESTINATION ${structured_include_dir}/noiseutils)

target_include_directories(${LIB_NAME}
        PUBLIC
        $<BUILD_INTERFACE:${structured_include_dir}>
        $<INSTALL_INTERFACE:include>
        PRIVATE
        ${CMAKE_SOURCE_DIR}
)

install(TARGETS ${LIB_NAME}
        EXPORT ${PROJECT_NAME}-targets
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
)

install(DIRECTORY ${structured_include_dir}/noiseutils DESTINATION include)
