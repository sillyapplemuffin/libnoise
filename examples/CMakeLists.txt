file(GLOB example_sources "*.cpp")
list(FILTER example_sources EXCLUDE REGEX "worms.cpp")

foreach(source_file ${example_sources})
    get_filename_component(exe_name ${source_file} NAME_WE)
    add_executable(${exe_name} ${source_file})
    target_link_libraries(${exe_name} noise::noise noiseutils::noiseutils)
    target_include_directories(${exe_name} PRIVATE ${CMAKE_SOURCE_DIR}/noiseutils)
endforeach()
