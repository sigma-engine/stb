cmake_minimum_required(VERSION 3.5)
project(libstb VERSION 1.0.0 LANGUAGES CXX)

file(GENERATE OUTPUT "${CMAKE_BINARY_DIR}/stb_image.cpp"
    CONTENT "#define STB_IMAGE_IMPLEMENTATION\n#include \"stb_image.h\"\n"
)

add_library(stb_image "${CMAKE_BINARY_DIR}/stb_image.cpp")

add_library(stb::stb_image ALIAS stb_image)

target_include_directories(stb_image
    PUBLIC
        $<INSTALL_INTERFACE:include>
        
    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        ${CMAKE_CURRENT_SOURCE_DIR}
)

include(GNUInstallDirs)
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/stb)

install(TARGETS stb_image
    EXPORT stb-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(FILES 
    stb.h
    stb_c_lexer.h
    stb_connected_components.h
    stb_divide.h
    stb_dxt.h
    stb_easy_font.h
    stb_herringbone_wang_tile.h
    stb_image.h
    stb_image_resize.h
    stb_image_write.h
    stb_leakcheck.h
    stb_perlin.h
    stb_rect_pack.h
    stb_sprintf.h
    stb_textedit.h
    stb_tilemap_editor.h
    stb_truetype.h
    stb_vorbis.c
    stb_voxel_render.h
    stretchy_buffer.h
DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/stb)

install(EXPORT stb-targets
  FILE
    stbTargets.cmake
  NAMESPACE
    stb::
  DESTINATION
    ${INSTALL_CONFIGDIR}
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/cmake/stbConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake
    INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake
    DESTINATION ${INSTALL_CONFIGDIR}
)

export(EXPORT stb-targets FILE ${CMAKE_CURRENT_BINARY_DIR}/stbTargets.cmake NAMESPACE stb::)

export(PACKAGE stb)
