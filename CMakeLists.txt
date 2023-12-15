project(QScintilla
    LANGUAGES CXX C
    )

find_package(Qt5 COMPONENTS Widgets OpenGL REQUIRED Designer PrintSupport)

message(STATUS "Found Qt5 version ${Qt5_VERSION}: ${Qt5_DIR}")

#add_custom_target(
#    clean_resource_file
#    COMMAND ${CMAKE_COMMAND} -E remove -f ${PROJECT_BINARY_DIR}/ui/QScintilla/qrc_font.o
#    COMMAND ${CMAKE_COMMAND} -E remove -f ${PROJECT_BINARY_DIR}/ui/QScintilla/qrc_icons.o
#)

file(GLOB_RECURSE source 
    designer/*.cpp
    designer/*.h
    example/*.cpp
    example/*.h
    scintilla/*.cpp
    scintilla/*.h
    src/*.cpp
    src/*.h
)
file(GLOB_RECURSE resource CONFIGURE_DEPENDS *.qrc)

qt5_add_big_resources(source ${bigresource})  # cihou msvc: fatal error C1060: compiler is out of heap space
qt5_add_resources(source ${resource})

add_executable(QScintilla ${source})
# Add a dependency to ensure that the resource file is deleted before being regenerated

# add_dependencies(QScintilla clean_resource_file)

set_property(TARGET QScintilla PROPERTY AUTOUIC ON)
set_property(TARGET QScintilla PROPERTY AUTOMOC ON)
set_property(TARGET QScintilla PROPERTY AUTORCC ON)
set_property(TARGET QScintilla PROPERTY WIN32_EXECUTABLE OFF)

#BEGIN PYTHON IMPORT
if (0)
    set(Python3_LIBRARIES C:/Users/Ada51/AppData/Local/Programs/Python/Python310/libs/python310_d.lib)
    set(Python3_INCLUDE_DIRS C:/Users/Ada51/AppData/Local/Programs/Python/Python310/include)

    message(STATUS "python3 libraries for QScintilla: ${Python3_LIBRARIES}")
    message(STATUS "python3 include directories for QScintilla: ${Python3_INCLUDE_DIRS}")
    target_link_libraries(QScintilla PRIVATE ${Python3_LIBRARIES})
    target_include_directories(QScintilla PRIVATE ${Python3_INCLUDE_DIRS})
endif()
#ENDIF

target_include_directories(QScintilla PRIVATE . ./src ./scintilla/include ./scintilla/lexlib ./scintilla/src)
target_link_libraries(QScintilla PRIVATE Qt5::Widgets Qt5::OpenGL Qt5::Designer Qt5::PrintSupport)

if (WIN32)
    target_link_libraries(QScintilla PRIVATE Dbghelp Version)
    target_link_libraries(QScintilla PRIVATE ws2_32.lib winmm.lib wldap32.lib Advapi32.lib Crypt32.lib)
endif()
