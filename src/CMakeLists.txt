if(ENABLE_THREAD_SUPPORT)
    list(APPEND QT_DEFINITIONS -DREENTRANT -DQT_THREAD_SUPPORT)
endif()

if(SYSTEM_ZLIB)
    include_directories(${ZLIB_INCLUDE_DIRS})
endif()

add_definitions(${QT_DEFINITIONS})

add_subdirectory(3rdparty)
add_subdirectory(moc)
add_subdirectory(tools)
add_subdirectory(kernel)
add_subdirectory(widgets)
add_subdirectory(dialogs)
add_subdirectory(iconview)
add_subdirectory(workspace)
add_subdirectory(network)
add_subdirectory(canvas)
add_subdirectory(table)
add_subdirectory(xml)

add_library(qt SHARED
    $<TARGET_OBJECTS:tools>
    $<TARGET_OBJECTS:kernel>
    $<TARGET_OBJECTS:widgets>
    $<TARGET_OBJECTS:dialogs>
    $<TARGET_OBJECTS:iconview>
    $<TARGET_OBJECTS:workspace>
    $<TARGET_OBJECTS:network>
    $<TARGET_OBJECTS:canvas>
    $<TARGET_OBJECTS:table>
    $<TARGET_OBJECTS:xml>
    ${THIRD_PARTY_OBJECTS}
)

target_link_libraries(qt
    ${ZLIB_LIBRARIES}
    ${PNG_LIBRARIES}
    ${FREETYPE_LIBRARIES}
    ${X11_LIBRARIES}
    ${MNG_LIBRARIES}
    ${JPEG_LIBRARIES}
    Threads::Threads
)

set_target_properties(qt PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    INTERFACE
)

install(TARGETS qt EXPORT Qt2Targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/Qt2/Qt2ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

export(EXPORT Qt2Targets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/Qt2/Qt2Targets.cmake"
    NAMESPACE Qt2::
)

configure_file(cmake/Qt2Config.cmake
    "${CMAKE_CURRENT_BINARY_DIR}/Qt2/Qt2Config.cmake"
    COPYONLY
)

#install(EXPORT Qt2Config DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/qt2/cmake)
#install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})


