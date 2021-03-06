# Install icons
#
# We put the bundled oxygen icon theme in DATA_INSTALL_DIR rather than ICON_INSTALL_DIR,
# in order to avoid conflicts. As we add this directory to XDG_DATA_DIRS at runtime,
# the bundled theme will be found by Qt only if there is no oxygen theme installed
# in system directories.
#
# The hicolor folder contains icons vital for the Quassel UI (such as in the channel and nick
# lists). It is installed into the system icon directory (in order to allow overriding) and
# additionally always embedded as a fallback resource.

if (WANT_MONO OR WANT_QTCLIENT)
    set(ICON_RCS ${CMAKE_CURRENT_SOURCE_DIR}/hicolor.qrc) # always embed those

    if (EMBED_DATA)
        if (WITH_OXYGEN)
            message(STATUS "Embedding bundled Oxygen icons")
            set(ICON_RCS ${ICON_RCS} ${CMAKE_CURRENT_SOURCE_DIR}/oxygen.qrc)
        endif()
    else()
        install(DIRECTORY hicolor DESTINATION ${ICON_INSTALL_DIR})
        if (WITH_OXYGEN)
            message(STATUS "Installing bundled Oxygen icons")
            install(DIRECTORY oxygen DESTINATION ${DATA_INSTALL_DIR}/quassel/icons)
        endif()
    endif()

    if (NOT WITH_OXYGEN)
        message(STATUS "Not installing bundled Oxygen icons")
    endif()

    set(CLIENT_RCS ${CLIENT_RCS} ${ICON_RCS} PARENT_SCOPE)
endif()

# Application icon

if (HAVE_KDE OR (UNIX AND NOT APPLE))
    install(FILES hicolor/48x48/apps/quassel.png DESTINATION ${ICON_INSTALL_DIR}/hicolor/48x48/apps)
    if (CMAKE_INSTALL_PREFIX STREQUAL "/usr")
        install(FILES hicolor/48x48/apps/quassel.png DESTINATION /usr/share/pixmaps)
    endif()
endif()
