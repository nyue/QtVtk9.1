cmake_minimum_required(VERSION 3.4)

project(QtVtkProj)
set(EXENAME QtVtk)

set(CMAKE_AUTOMOC ON)

if (USE_QT5)
  find_package(Qt5Quick REQUIRED)
  find_package(Qt5Widgets REQUIRED)
  find_package(Qt5Qml REQUIRED)
  find_package(Qt5QuickControls2 REQUIRED)
  find_package(Qt5Quick REQUIRED)
  set (REQUIRED_QT_LIBRARIES
	Qt5::Quick Qt5::Widgets Qt5::Qml
	)
else ()
  find_package(Qt6Quick REQUIRED)
  find_package(Qt6Widgets REQUIRED)
  find_package(Qt6Qml REQUIRED)
  find_package(Qt6QuickControls2 REQUIRED)
  find_package(Qt6Quick REQUIRED)
  set (REQUIRED_QT_LIBRARIES
	Qt6::Quick Qt6::Widgets Qt6::Qml
	)
endif()

find_package(VTK REQUIRED)

message("VTK_AUTOINIT_INCLUDE ${VTK_AUTOINIT_INCLUDE}")
message("VTK_PREFIX_PATH ${VTK_PREFIX_PATH}")

# Headers directories
include_directories(${PROJECT_SOURCE_DIR}
  ${PROJECT_SOURCE_DIR}/src
  )

###########
# Targets #
###########

# Add QML files to project tree in Qt Creator
file(GLOB QML_DESKTOP "*.qml")
message("NICHOLAS QML_DESKTOP = ${QML_DESKTOP}")

add_custom_target(QML_files SOURCES ${QML_DESKTOP})

# Sources
set (SOURCES
  main.cpp
  qml.qrc
  qtvtkitem.h
  qtvtkitem.cpp
  vtkinteractorstyletrackballcamerastyle2.h
  vtkinteractorstyletrackballcamerastyle2.cpp
  CommandModel.cpp
  CommandModel.h
  CommandModelAdd.cpp
  CommandModelAdd.h
  Model.cpp
  Model.h
  ProcessingEngine.cpp
  ProcessingEngine.h
  qquickvtkrenderitem.h
  qquickvtkrenderitem.cpp
  )

add_definitions(-DQT_NO_VERSION_TAGGING)    #compiler errorundefined reference to `qt_version_tag@Qt_5.15'

# Qt Resources
if (USE_QT5)
  qt5_add_resources(RESOURCES qml.qrc)
else()
  qt6_add_resources(RESOURCES qml.qrc)
endif()

# Build executable
if (WIN32)
  add_executable(${EXENAME} WIN32 ${HEADERS} ${SOURCES} ${RESOURCES})
else()
  add_executable(${EXENAME} ${HEADERS} ${SOURCES} ${RESOURCES})
endif()
#set (EXCLUDE_DIR "/usr/lib/x86_64-linux-gnu/")      #compiler errorundefined reference to `qt_version_tag@Qt_5.15'

message("V ${VTK_LIBRARIES}")

# Link to libraries
target_link_libraries(${EXENAME}
  ${REQUIRED_QT_LIBRARIES}
  ${VTK_LIBRARIES}
  ${SYSTEM_LIBS}
  )
