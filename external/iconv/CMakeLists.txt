# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

add_library(external_iconv INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_iconv ALIAS external_iconv)

if (APPLE AND NOT build_osx)
    target_link_libraries(external_iconv
    INTERFACE
        /usr/local/macos/lib/libiconv.a
    )
elseif (APPLE)
    target_link_libraries(external_iconv
    INTERFACE
        /usr/local/lib/libiconv.a
    )
endif()
