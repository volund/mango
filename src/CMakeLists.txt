cmake_minimum_required(VERSION 2.8)
project(Mango_libs)

# Version number
set(MANGO_VERSION_MAJOR 1)
set(MANGO_VERSION_MINOR 1)

# Configure header 
configure_file (
  "${PROJECT_SOURCE_DIR}/MangoConfig.h.in"
  "${PROJECT_SOURCE_DIR}/../include/MangoConfig.h"
  )



find_package(OpenGL REQUIRED)

include_directories(${OPENGL_INCLUDE_DIR})

include_directories("${PROJECT_SOURCE_DIR}/../include")

file(GLOB Mango_core_src "${PROJECT_SOURCE_DIR}/core/*.cpp")
set(Mango_core_src "${PROJECT_SOURCE_DIR}/mango.cpp" ${Mango_core_src})
file(GLOB Mango_draw_src "${PROJECT_SOURCE_DIR}/draw/*.cpp")
file(GLOB Mango_geometry_src "${PROJECT_SOURCE_DIR}/geometry/*.cpp")
file(GLOB Mango_glut_src "${PROJECT_SOURCE_DIR}/glut/*.cpp")

# Add -fPIC on 64bit machines
if (UNIX AND NOT WIN32)
  find_program(CMAKE_UNAME uname /bin /usr/bin /usr/local/bin )
  if (CMAKE_UNAME)
    exec_program(uname ARGS -m OUTPUT_VARIABLE CMAKE_SYSTEM_PROCESSOR)
    set(CMAKE_SYSTEM_PROCESSOR ${CMAKE_SYSTEM_PROCESSOR} CACHE INTERNAL "processor type(i386 and x86_64)")
    if (CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64")
      add_definitions(-fPIC)
    endif (CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64")
  endif(CMAKE_UNAME)
endif(UNIX AND NOT WIN32)

add_library(Mango_core ${Mango_core_src})
target_link_libraries(Mango_core ${OPENGL_LIBRARIES})

add_library(Mango_geometry ${Mango_geometry_src})
target_link_libraries(Mango_geometry ${OPENGL_LIBRARIES})

add_library(Mango_draw ${Mango_draw_src})
target_link_libraries(Mango_draw ${OPENGL_LIBRARIES})

## Mango_on_GLUT is optional

find_package(GLUT)
if(${GLUT_FOUND})
  include_directories(${GLUT_INCLUDE_DIR})
  add_library(Mango_glut ${Mango_glut_src})
  target_link_libraries(Mango_glut ${GLUT_LIBRARIES})
else()
  message("GLUT not found, not building Mango_glut")
endif()



## Build the Python Bindings
find_package(PythonLibs 3.2)

if(${PYTHONLIBS_FOUND})
  include_directories(${PYTHON_INCLUDE_PATH})

  file(GLOB Mango_py_src "${PROJECT_SOURCE_DIR}/mangopy/*.cpp")
  add_library(Mango_py ${Mango_py_src})
  target_link_libraries(Mango_py ${PYTHON_LIBRARIES} Mango_core Mango_draw)

  file(GLOB Mango_pygeometry_src "${PROJECT_SOURCE_DIR}/pygeometry/*.cpp")

  add_library(Mango_pygeometry SHARED ${Mango_pygeometry_src})
  set_target_properties(Mango_pygeometry PROPERTIES PREFIX "")
  if(WIN32)
     set_target_properties(Mango_pygeometry PROPERTIES SUFFIX ".pyd")
  endif(WIN32)
  set_target_properties(Mango_pygeometry PROPERTIES OUTPUT_NAME "Geometry")
  target_link_libraries(Mango_pygeometry ${PYTHON_LIBRARIES} Mango_core Mango_geometry Mango_py)

if(${GLUT_FOUND})
  include_directories(${GLUT_INCLUDE_DIR})
  add_executable(mango "${PROJECT_SOURCE_DIR}/mangopy_on_glut/mangopy_on_glut.cpp")
  target_link_libraries(mango ${GLUT_LIBRARIES} Mango_core Mango_draw Mango_glut Mango_py)
else()
  message("GLUT not found, not building the mango executable")
endif()

else()
  message("Not building the Python Bindings. No Python installation has been found.")
endif()




# Install
install(TARGETS mango DESTINATION bin)
install(TARGETS Mango_pygeometry DESTINATION lib/mangopy/) 

file(GLOB Mango_py_samples "${PROJECT_SOURCE_DIR}/../samples_mangopy/mangopy/*.py")
install(FILES ${Mango_py_samples} DESTINATION share/mangopy/samples/)

install(TARGETS Mango_core DESTINATION lib)
install(TARGETS Mango_geometry DESTINATION lib)
install(TARGETS Mango_draw DESTINATION lib)
install(TARGETS Mango_glut DESTINATION lib)
install(TARGETS Mango_py DESTINATION lib)
