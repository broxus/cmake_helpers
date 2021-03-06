# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

add_library(external_minizip INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_minizip ALIAS external_minizip)

if (DESKTOP_APP_USE_PACKAGED)
    find_package(PkgConfig REQUIRED)
    pkg_check_modules(MINIZIP REQUIRED minizip)

    target_include_directories(external_minizip INTERFACE ${MINIZIP_INCLUDE_DIRS})
    target_link_libraries(external_minizip INTERFACE ${MINIZIP_LIBRARIES})
elseif (NOT WIN32)
    add_library(external_minizip_bundled STATIC)
    init_target(external_minizip_bundled "(external)")

    set(minizip_loc ${third_party_loc}/minizip)

    target_sources(external_minizip_bundled
    PRIVATE
        ${minizip_loc}/crypt.h
        ${minizip_loc}/ioapi.c
        ${minizip_loc}/ioapi.h
        ${minizip_loc}/zip.c
        ${minizip_loc}/zip.h
        ${minizip_loc}/unzip.c
        ${minizip_loc}/unzip.h
    )

    target_include_directories(external_minizip_bundled SYSTEM
    INTERFACE
        ${minizip_loc}
    )

    target_link_libraries(external_minizip INTERFACE external_minizip_bundled)
else()
    target_include_directories(external_minizip SYSTEM
    INTERFACE
        ${libs_loc}/zlib/contrib/minizip
    )
endif()
