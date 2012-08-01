Building on OSX
===============

Installing Dependencies
-----------------------

Follow the setup steps for building Mango simulations in C++ described
in :ref:`setup-osx-cpp` (it is not necessary to run the Mango installer as
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

Open a terminal and cd into the directory containing the source, which will
be assumed to be *~/src/mango*. Although not strictly
necessary we will also create a build directory and configure/build
the project there (if you prefer not to do this simply skip those
steps and adjust commands appropriately):

  .. code-block:: bash

     $ cd ~/src/mango
     $ mkdir build-osx
     $ cd build-osx

Next CMake must be run. This is usually accomplished by executing:

  .. code-block:: bash
     
     $ cmake ..

CMake will attempt to find the relevant Python, OpenGL and GLUT
dependencies. If this works, you should see output similar to:

  .. code-block:: bash

     ...    
     -- Found OpenGL: /System/Library/Frameworks/OpenGL.framework  
     -- Found GLUT: -framework GLUT  
     -- Found PythonLibs: /usr/lib/libpython3.2.dylib (found suitable version "3.2.2", required is "3.2") 
     -- Configuring done
     -- Generating done
     -- Build files have been written to: ~/source/mango/build-osx

If this fails, you must resolve whichever problem is preventing correct configuration
before proceeding. Once configured correctly, Mango can be built with:

  .. code-block:: bash

     $ make
