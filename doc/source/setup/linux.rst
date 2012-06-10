Setup on Linux
==============

Installing Mango on linux requires building it from source, by
executing the standard *cmake*, *make*, *make install*
commands. 

Installing Dependencies
-----------------------

Mango depends on GLUT and Python and requires CMake for compilation. 
While not strictly necessary, these instructions also assume that
you have git installed. On Ubuntu, these dependencies may be 
installed by issuing the following command:

  .. code-block:: bash
     
     $ sudo apt-get install g++ freeglut3-dev python3.2-dev cmake git

Installing dependencies on other distributions may be slightly
different, but should follow similar lines. 

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

     -- Found X11: /usr/lib/libX11.so
     -- Found OpenGL: /usr/lib/libGL.so  
     -- Found GLUT: /usr/lib/libglut.so  
     -- Could NOT find PythonLibs (missing:  PYTHON_LIBRARIES PYTHON_INCLUDE_DIRS) (Required is at least version "3.2")
     Not building the Python Bindings. No Python installation has been found.
     -- Configuring done
     -- Generating done
     -- Build files have been written to: /home/volund/mango/build-bla

In cases such as these whichever problem is preventing correct configuration must 
be resolved before proceeding.

Once configured successfully, Mango can be built and installed with:

  .. code-block:: bash

     $ make
     $ sudo make install

You are now ready to run simulations written in Python (see
:doc:`/samples`). To do so, type:

  .. code-block:: bash

     $ mango path/to/script.py

By default, Mango will install the *mango* binary at */opt/mango/<version>/* and include
the following components:
    
    * **core**:      
        A directory containing the mango executable. The installer will
        automatically link to it in /usr/bin/mango.

    * **doc**:    
        A copy of the documentation.

    * **sample**:
        Sample mango scripts.

    * **script**:
        A directory whose contents are available directly to
        Mango. Scripts or libraries places in this directory can be
        imported by any mango script. Note that it is not intended for
        all of your scripts to live in this directory, only those that
        should be accessible on a system-wide scale.

    * **include**:
        C++ header files. These should be used in conjunction with the
        libraries in *lib*.

    * **lib**:
        Static libraries for mango, mango_on_glut, geometry and
        mangopy. These may linked against when developing simulations
        in C++.
	
You are also ready to build simulation in C++. See
:doc:`/cpp/rapid-simulation-development` for further instructions.
