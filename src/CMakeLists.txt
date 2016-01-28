set(${PROJECT_NAME}_HDRS
    dialog/mainwindow.h
    vpninfo.h
    storage.h
    dialog/editdialog.h
    common.h
    keypair.h
    key.h
    cert.h
    dialog/logdialog.h
    gtdb.h
    dialogs.h
    cryptdata.h
)
set(${PROJECT_NAME}_SRCS
    main.cpp
    dialog/mainwindow.cpp
    vpninfo.cpp
    storage.cpp
    dialog/editdialog.cpp
    keypair.cpp
    key.cpp
    cert.cpp
    dialog/logdialog.cpp
    gtdb.cpp
    cryptdata.cpp
)
set(${PROJECT_NAME}_FORMS
    dialog/mainwindow.ui
    dialog/editdialog.ui
    dialog/logdialog.ui
)

set(${PROJECT_NAME}_RESOURCES
    ${PROJECT_NAME}.qrc
)

configure_file(
    config.cpp.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.cpp
    @ONLY
)
list(APPEND ${PROJECT_NAME}_SRCS
    ${CMAKE_CURRENT_BINARY_DIR}/config.cpp
    config.cpp.in
    config.h
)

if(WIN32 AND MINGW)
    configure_file(
        ${PROJECT_NAME}.rc.in
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.rc
        @ONLY
    )
    list(APPEND ${PROJECT_NAME}_RESOURCES
        ${PROJECT_NAME}.rc.in
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.rc
    )
endif()

add_executable(${PROJECT_NAME} WIN32
	${${PROJECT_NAME}_SRCS}
	${${PROJECT_NAME}_HDRS}
	${${PROJECT_NAME}_FORMS}
	${${PROJECT_NAME}_RESOURCES}
)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 98)

target_link_libraries (${PROJECT_NAME}
	Qt5::Core
	Qt5::Gui
	Qt5::Widgets
	Qt5::Network

	# system libraries
	wsock32

	# 3rd party libraries
	libgmp.dll.a
	libgnutls-xssl.dll.a
	libgnutls.dll.a
	libhogweed.dll.a
	libnettle.dll.a
	libopenconnect.dll.a
	libp11-kit.dll.a
	libstoken.dll.a
	libxml2.dll.a
	libz.dll.a
)