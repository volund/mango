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

file(GLOB mango_core_src "${PROJECT_SOURCE_DIR}/core/*.cpp")
set(mango_core_src "${PROJECT_SOURCE_DIR}/mango.cpp" ${mango_core_src})
file(GLOB mango_draw_src "${PROJECT_SOURCE_DIR}/draw/*.cpp")
file(GLOB mango_geometry_src "${PROJECT_SOURCE_DIR}/geometry/*.cpp")
file(GLOB mango_onglut_src "${PROJECT_SOURCE_DIR}/glut/*.cpp")

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

add_library(mango_core ${mango_core_src} ${mango_draw_src})
target_link_libraries(mango_core ${OPENGL_LIBRARIES})

add_library(mango_geometry ${mango_geometry_src})
target_link_libraries(mango_geometry ${OPENGL_LIBRARIES})

##add_library(mango_draw ${mango_draw_src})
##target_link_libraries(mango_draw ${OPENGL_LIBRARIES})

## mango_onglut is optional

find_package(GLUT)

## It seems that on win32 it is necessary to link against
## both freeglut.a and freeglut.dll (the former contains
## methods that the latter depends on; a true static 
## linking against freeglut would involve freeglut_static.a).
## As of version CMake 2.8 FindGLUT.cmake doesn't handle this
## well and does not link against freeglut.a when it finds 
## the dll. Check for this condition and fix accordingly
if (${WIN32})
  if (NOT DEFINED DISABLE_FREEGLUT_DLL_FIX)
    SET(DEFINED_DISABLE_FREEGLUT_DLL_FIX 0)
  endif()
  if (${DISABLE_FREEGLUT_DLL_FIX})
    message("-- Skipping freeglut.dll fix on WIN32 due to presence of flag")
  else()
    if (${GLUT_LIBRARIES} MATCHES "freeglut\\.dll$")
      message("-- Appending 'freeglut' to GLUT_LIBRARIES for correct WIN32 linking against freeglut.dll (pass -DDISABLE_FREEGLUT_DLL_FIX=1 to disable)")
      SET(GLUT_LIBRARIES 
          ${GLUT_LIBRARIES} 
          freeglut)
      message("-- New GLUT_LIBRARIES value: ${GLUT_LIBRARIES}")
    endif()
  endif()
endif()

if(${GLUT_FOUND})
  include_directories(${GLUT_INCLUDE_DIR})
  add_library(mango_onglut ${mango_onglut_src})
  target_link_libraries(mango_onglut ${GLUT_LIBRARIES})
else()
  message("GLUT not found, not building mango_onglut")
endif()



## Build the Python Bindings
find_package(PythonLibs 3.2)

if(${PYTHONLIBS_FOUND})
  include_directories(${PYTHON_INCLUDE_PATH})

  file(GLOB mango_py_src "${PROJECT_SOURCE_DIR}/mangopy/*.cpp")
  add_library(mango_py ${mango_py_src})
  target_link_libraries(mango_py ${PYTHON_LIBRARIES} mango_core)

  file(GLOB mango_pygeometry_src "${PROJECT_SOURCE_DIR}/pygeometry/*.cpp")

  add_library(mango_pygeometry SHARED ${mango_pygeometry_src})
  set_target_properties(mango_pygeometry PROPERTIES PREFIX "")
  if(WIN32)
     set_target_properties(mango_pygeometry PROPERTIES SUFFIX ".pyd")
  endif(WIN32)
  set_target_properties(mango_pygeometry PROPERTIES OUTPUT_NAME "Geometry")
  target_link_libraries(mango_pygeometry ${PYTHON_LIBRARIES} mango_core mango_geometry mango_py)

if(${GLUT_FOUND})
  include_directories(${GLUT_INCLUDE_DIR})
  add_executable(mango "${PROJECT_SOURCE_DIR}/mangopy_on_glut/mangopy_on_glut.cpp")
  target_link_libraries(mango ${GLUT_LIBRARIES} mango_core mango_onglut mango_py)
else()
  message("GLUT not found, not building the mango executable")
endif()

else()
  message("Not building the Python Bindings. No Python installation has been found.")
endif()




# Install
if(${PYTHONLIBS_FOUND})
  install(TARGETS mango DESTINATION bin)

  install(TARGETS mango_pygeometry DESTINATION lib/mangopy/) 

  file(GLOB mango_py_samples "${PROJECT_SOURCE_DIR}/../samples_mangopy/mangopy/*.py")
  install(FILES ${mango_py_samples} DESTINATION share/mangopy/samples/)
  
  install(TARGETS mango_py DESTINATION lib)
endif()

install(TARGETS mango_core DESTINATION lib)
install(TARGETS mango_geometry DESTINATION lib)
install(TARGETS mango_onglut DESTINATION lib)

file(GLOB base_include "${PROJECT_SOURCE_DIR}/../include/*.h")
install(FILES ${base_include} DESTINATION include/mango)

file(GLOB mango_include "${PROJECT_SOURCE_DIR}/../include/mango/*.h")
install(FILES ${mango_include} DESTINATION include/mango/)

file(GLOB mango_core_include "${PROJECT_SOURCE_DIR}/../include/mango/core/*.h")
install(FILES ${mango_core_include} DESTINATION include/mango/core)

file(GLOB mango_draw_include "${PROJECT_SOURCE_DIR}/../include/mango/draw/*.h")
install(FILES ${mango_draw_include} DESTINATION include/mango/draw)

file(GLOB mango_geometry_include "${PROJECT_SOURCE_DIR}/../include/mango/geometry/*.h")
install(FILES ${mango_geometry_include} DESTINATION include/mango/geometry)

file(GLOB mango_glut_include "${PROJECT_SOURCE_DIR}/../include/mango/glut/*.h")
install(FILES ${mango_glut_include} DESTINATION include/mango/glut)

file(GLOB mango_mangopy_include "${PROJECT_SOURCE_DIR}/../include/mango/mangopy/*.h")
install(FILES ${mango_mangopy_include} DESTINATION include/mango/mangopy)

file(GLOB mango_pygeometry_include "${PROJECT_SOURCE_DIR}/../include/mango/pygeometry/*.h")
install(FILES ${mango_pygeometry_include} DESTINATION include/mango/pygeometry)
