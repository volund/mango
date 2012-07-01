Building on Windows
===================

Installing Dependencies
-----------------------

Follow the setup steps for building Mango simulations in C++ described
in :ref:`setup-windows-cpp` (it is not necessary to run the Mango installer as
described in the first section). Make sure to install an appropriate
version of Python.

Obtaining the Source
--------------------
Mango's source code can be obtained by downloading an archived copy
or by cloning the git repository. For the latest archive or repository
address check the home page. At the time of writing the following 
command clones the repository in the current directory:

  .. code-block:: bash
   
     $ git clone https://volund@github.com/volund/mango.git .
 
Configuring, Building and Installing
------------------------------------

Open a MinGW shell (for instance by selecting "MinGW > MinGW Shell"
from the Program Files menu) and cd into the directory containing the
source, which will be assumed to be */c/src/mango*. Although not strictly
necessary we will also create a build directory and configure/build
the project there (if you prefer not to do this simply skip those
steps and adjust commands appropriately):

  .. code-block:: bash

     $ cd C:\\src\\mango
     $ mkdir build-windows
     $ cd build-windows

Next CMake must be run. If during installation it was added to the
path CMake may be invoked simply as *cmake* - otherwise the full
path to the executable must be used. We will assume it was not added
to the path and that it was installed to *C:/Program Files/CMake 2.8*.
Then once we have changed into the
build directory running CMake may usually be accomplished by running:

  .. code-block:: bash
     
     $ /c/Program\ Files/Cmake\ 2.8/bin/cmake -G "MSYS Makefiles" ..

CMake will attempt to find the relevant Python, OpenGL and GLUT
dependencies. If this works, you should see output similar to:
  
  .. code-block:: bash

     ...    
     -- Detecting CXX compiler ABI info
     -- Detecting CXX compiler ABI info - done
     -- Found OpenGL: opengl32
     -- Found GLUT: c:/WINDOWS/system32/freeglut.dll
     -- Appending 'freeglut' to GLUT_LIBRARIES for correct WIN32 linking against free
     glut.dll (pass -DDISABLE_FREEGLUT_DLL_FIX=1 to disable)
     -- New GLUT_LIBRARIES value: c:/WINDOWS/system32/freeglut.dll;freeglut
     -- Found PythonLibs: C:/Python32/libs/libpython32.a (found suitable version "3.2
     .3", required is "3.2")
     -- Configuring done
     -- Generating done
     -- Build files have been written to: C:/src/mango/build

If this fails, you will see output similar to (in this case due to a failure
to find GLUT):

  .. code-block:: bash

     ..
     -- Found OpenGL: opengl32
     -- Could NOT find GLUT (missing:  GLUT_INCLUDE_DIR)
     GLUT not found, not building mango_onglut
     -- Found PythonLibs: C:/Python32/libs/libpython32.a (found suitable version "3.
     .3", required is "3.2")
     GLUT not found, not building the mango executable
     
     -- Configuring incomplete, errors occurred!

In cases such as these whichever problem is preventing correct configuration must 
be resolved before proceeding. For instance, on a particular Windows XP build 
machine the following expanded command is used to resolve the above inability to 
find the GLUT include directory:

  .. code-block:: bash

     $ /c/Program\ Files/Cmake\ 2.8/bin/cmake -G "MSYS Makefiles" -DGLUT_ROOT_PATH="C:/MinGW/" ..


Once configured successfully, Mango can be built with:

  .. code-block:: bash

     $ make
