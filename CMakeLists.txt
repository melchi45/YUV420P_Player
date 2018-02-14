cmake_minimum_required(VERSION 2.8)

set ( PROJECT_NAME YUV420Player )
set ( TINYLIB tinylib )

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})
message ( "CMAKE_MODULE_PATH = ${CMAKE_MODULE_PATH}" )

if ( CMAKE_COMPILER_IS_GNUCXX )
   set ( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpermissive" )
endif ()

project( ${PROJECT_NAME} )

set ( ${PROJECT_NAME}_HOME ${CMAKE_CURRENT_SOURCE_DIR} )
message("SourceDir: " ${${PROJECT_NAME}_HOME} )

set ( ${TINYLIB}_HOME ${${PROJECT_NAME}_HOME}/${TINYLIB} )
set ( ${TINYLIB}_INCLUDE ${${PROJECT_NAME}_HOME}/${TINYLIB}/src )
message("Tinylib: " ${${TINYLIB}_INCLUDE} )

find_package(FFmpeg COMPONENTS AVCODEC AVFORMAT AVUTIL AVDEVICE REQUIRED)

if(FFMPEG_FOUND)
  message("Found FFMPEG/LibAV libraries")
  message("FFMPEG_INCLUDE_DIRS = ${FFMPEG_INCLUDE_DIRS} ")
  message("FFMPEG_LIBRARIES = ${FFMPEG_LIBRARIES} ")
else()
    message(FATAL_ERROR "FFMPEG not found")
endif()

find_package(OpenGL REQUIRED)
#find_package(PkgConfig REQUIRED)

find_package(glfw3 REQUIRED)


#message("glfw include dir: ${GLFW3_LIBRARIES}")
message("glfw libraries: ${GLFW3_LIBRARIES}")

find_package(PNG REQUIRED)

set(CXX_SRC_EXT_PATTERN ".cpp")
set(C_SRC_EXT_PATTERN ".c")
set(HDR_EXT_PATTERN ".h")

# get all *.cpp files from src to build target
file(GLOB SRC_FILES ${PROJECT_SOURCE_DIR}/src/*${CXX_SRC_EXT_PATTERN} ${PROJECT_SOURCE_DIR}/src/*${C_SRC_EXT_PATTERN})
file(GLOB HDR_FILES ${PROJECT_SOURCE_DIR}/src/*${HDR_EXT_PATTERN})
# makes source and header files group
source_group("Source Files" FILES ${SRC_FILES})
source_group("Header Files" FILES ${HDR_FILES})

# build target
add_executable(${PROJECT_NAME} ${SRC_FILES} ${HDR_FILES})
target_include_directories( ${PROJECT_NAME} PRIVATE ${FFMPEG_INCLUDE_DIRS} )
target_include_directories(${PROJECT_NAME} PRIVATE ${${TINYLIB}_INCLUDE} )
target_include_directories(${PROJECT_NAME} PRIVATE glfw)

target_link_libraries(${PROJECT_NAME} ${FFMPEG_LIBRARIES} )
target_link_libraries(${PROJECT_NAME} PRIVATE ${GLFW3_LIBRARIES})
target_link_libraries(${PROJECT_NAME} PRIVATE PNG::PNG)