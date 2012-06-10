Setup on Linux
==============

Installing Mango on linux requires building it from source, by
executing the standard *cmake*, *make*, *make install*
commands. 

Installing Dependencies
-----------------------

Mango depends on GLUT and Python and requires CMake for compilation. 
While not strictly necessary, these instructions also assume that
you have git installed. On Ubuntu, these primary dependencies may be 
installed by issuing the following command:

  .. code-block:: bash
     
     $ sudo apt-get install g++ freeglut3-dev python3.2-dev cmake git

Installing dependencies on other distributions may be slightly
different, but should follow similar lines. In addition to these, other
secondary dependencies may be necessary. For instance at the time of
writing on Kubuntu 12.1 the following are also required:

  .. code-block:: bash
     
     $ sudo apt-get install make libxmu-dev libxi-dev

The need for any such secondary dependencies usually becomes evident
later if compilation fails with only the above primary ones installed.

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

Open a terminal and change into the directory containing the source, 
which will be assumed to be *~/src/mango/*. Although not strictly
necessary, we will also create a build directory and configure/build
the project there (if you prefer not to do this simply skip those
steps and adjust commands appropriately):

  .. code-block:: bash

     $ cd ~/src/mango
     $ mkdir build
     $ cd build

On most platforms, once we have changed into the build directory
running CMake is as simple as:

  .. code-block:: bash
     
     $ cmake ..

CMake will attempt to find the relevant Python, OpenGL and GLUT
dependencies. If this works, you should see output similar to:
  
  .. code-block:: bash

     ...    
     -- Found X11: /usr/lib/libX11.so
     -- Found OpenGL: /usr/lib/libGL.so  
     -- Found GLUT: /usr/lib/libglut.so  
     -- Found PythonLibs: /usr/lib/libpython3.2mu.so (found suitable version "3.2.3", required is "3.2") 
     -- Configuring done
     -- Generating done
     -- Build files have been written to: ~/src/mango/build

If this fails, you will see output similar to (in this case due to a failure
to find Python):

  .. code-block:: bash

     ...
     -- Found X11: /usr/lib/libX11.so
     -- Found OpenGL: /usr/lib/libGL.so  
     -- Found GLUT: /usr/lib/libglut.so  
     -- Could NOT find PythonLibs (missing:  PYTHON_LIBRARIES PYTHON_INCLUDE_DIRS) (Required is at least version "3.2")
     Not building the Python Bindings. No Python installation has been found.
     -- Configuring done
     -- Generating done
     -- Build files have been written to: ~/src/mango/build

In cases such as these whichever problem is preventing correct configuration must 
be resolved before proceeding. For instance, on a particular Kubuntu 12.1 build 
machine the following expanded command is used to resolve the above inability to 
find python:

  .. code-block:: bash

     cmake .. -DPYTHON_INCLUDE_PATH=/usr/include/python3.2mu -DPYTHON_LIBRARY=/usr/lib/libpython3.2mu.so

Once configured successfully, Mango can be built and installed with:

  .. code-block:: bash

     $ make
     $ sudo make install

You are now ready to run simulations written in Python (see
:doc:`/samples`). To do so, type:

  .. code-block:: bash

     $ mango path/to/script.py

Mango installations install the following components relative to an
installation prefix which defaults to **/usr/local/**:
    
    * **bin/mango**
        The mango executable

    * **lib/mangopy/**:
        Directory intended to hold Mango modules written in Python.
        This location is automatically added to Mango's module search
        path, so that modules placed here are globally available to
        all Mango scripts written in Python. Currently only one module
        is pre-installed here: Geometry.so

    * **share/mangopy/samples/**:
        Sample Mango scripts written in Python

    * **include/mango/**:
        Mango include headers for use when developing simulations in C++

    * **lib/**:
        Static libraries for mango, mango_on_glut, geometry and
        mangopy. These may linked against when developing simulations
        in C++. Currently the following libraries are installed:
        libmango_core.a, libmango_glut.a, libmango_py.a, 
        libmango_geometry.a
	
You are also ready to build simulation in C++. See
:doc:`/cpp/rapid-simulation-development` for further instructions.
