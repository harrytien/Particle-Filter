CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT(particle_filter)

SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")

FIND_PACKAGE(OpenCV REQUIRED)
FIND_PACKAGE(GSL REQUIRED)

INCLUDE_DIRECTORIES(${GSL_INCLUDE_DIRS} ${GSLCBLAS_INCLUDE_DIRS})
SET(GSLLIB ${LIBS} ${GSL_LIBRARIES} ${GSLCBLAS_LIBRARIES})

# set constant paths to video and cascades.xml file
SET(VIDEO_PATH "${CMAKE_SOURCE_DIR}/data/fussgaengerzone.mp4")
SET(CASCADE_XML_PATH "${CMAKE_SOURCE_DIR}/data/cascades.xml")
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/src/constants.h 
	${CMAKE_CURRENT_SOURCE_DIR}/src/constants.h @ONLY)

# list of sources
SET(SOURCES
src/main.cpp
src/constants.h
src/colorFeatures.h src/colorFeatures.cpp
src/adaboostDetect.h src/adaboostDetect.cpp
src/particleFilter.h src/particleFilter.cpp
src/tracker.h src/tracker.cpp
)

IF (APPLE)
	ADD_EXECUTABLE(main MACOSX_BUNDLE ${SOURCES})
ELSE ()
	ADD_EXECUTABLE(main ${SOURCES})
ENDIF ()

# external libs
TARGET_LINK_LIBRARIES(main ${OpenCV_LIBS} ${GSLLIB})
LINK_LIBRARIES(${GSLLIB})