set(PROJECT_NAME libnoise)
set(LIB_NAME noise)

file(GLOB libSrcs_base "src/*.cpp")
file(GLOB libSrcs_model "src/model/*.cpp")
file(GLOB libSrcs_module "src/module/*.cpp")

set(libSrcs ${libSrcs_base} ${libSrcs_model} ${libSrcs_module})

if(win32 AND BUILD_SHARED_LIBS)
    list(APPEND libSrcs "src/win32/dllmain.cpp")
endif()

add_library(${LIB_NAME} ${libSrcs})
add_library(noise::${LIB_NAME} ALIAS ${LIB_NAME})

set(structured_include_dir ${CMAKE_CURRENT_BINARY_DIR}/include)

# Create directory structure
file(MAKE_DIRECTORY ${structured_include_dir}/noise/model)
file(MAKE_DIRECTORY ${structured_include_dir}/noise/module)

# Copy headers
file(GLOB header_files_base "src/*.h")
file(COPY ${header_files_base} DESTINATION ${structured_include_dir}/noise)
file(GLOB header_files_model "src/model/*.h")
file(COPY ${header_files_model} DESTINATION ${structured_include_dir}/noise/model)
file(GLOB header_files_module "src/module/*.h")
file(COPY ${header_files_module} DESTINATION ${structured_include_dir}/noise/module)

target_include_directories(${LIB_NAME}
    PRIVATE
        src
    PUBLIC
        $<BUILD_INTERFACE:${structured_include_dir}>
        $<INSTALL_INTERFACE:include>
)

if(win32 AND BUILD_SHARED_LIBS)
    target_compile_definitions(${LIB_NAME} PRIVATE NOISE _BUILD_DLL)
    set_target_properties(${LIB_NAME} PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

install(TARGETS ${LIB_NAME}
    EXPORT ${PROJECT_NAME}-targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY ${structured_include_dir}/noise DESTINATION include)
