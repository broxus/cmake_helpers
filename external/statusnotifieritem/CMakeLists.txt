# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

add_library(external_statusnotifieritem OBJECT)
add_library(desktop-app::external_statusnotifieritem ALIAS external_statusnotifieritem)

set(statusnotifieritem_loc ${third_party_loc}/statusnotifieritem)

nice_target_sources(external_statusnotifieritem ${statusnotifieritem_loc}
PRIVATE
    dbustypes.cpp
    dbustypes.h
    statusnotifieritem.cpp
    statusnotifieritem.h
)

qt5_add_dbus_adaptor(sni_adaptor
    ${statusnotifieritem_loc}/org.kde.StatusNotifierItem.xml
    ${statusnotifieritem_loc}/statusnotifieritem.h
    StatusNotifierItem
)

set_target_properties(external_statusnotifieritem PROPERTIES AUTOMOC ON)

target_sources(external_statusnotifieritem PRIVATE ${sni_adaptor})

target_include_directories(external_statusnotifieritem
PUBLIC
    ${statusnotifieritem_loc}
    "${CMAKE_CURRENT_BINARY_DIR}"
)

target_link_libraries(external_statusnotifieritem
PUBLIC
    desktop-app::external_dbusmenu_qt
    desktop-app::external_qt
)
