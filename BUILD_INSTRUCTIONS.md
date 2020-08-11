# Requirements

StarSpace builds on modern Mac OS, Windows, and Linux distributions. Since it uses C++11 features, it requires a compiler with good C++11 support. These include :

* (gcc-4.6.3 or newer), (Visual Studio 2015 or newer), or (clang-3.3 or newer) 

All of the instructions below refering to Windows consider the use of Visual Studio as the compiler, since you also might need to update some project properties.

Other requirements:    
* <a href=http://www.boost.org/>Boost</a>
* Make (Unix)
* (Optional) <a href=https://github.com/google/googletest>Google Test</a>


# Boost Installation
### Unix
You need to install boost library and specify its path in makefile in order to run StarSpace. Basically:

    $wget https://dl.bintray.com/boostorg/release/1.63.0/source/boost_1_63_0.zip
    $unzip boost_1_63_0.zip
    $sudo mv boost_1_63_0 /usr/local/bin
    
Note: All configurations are prepared for boost 1.63.0. If you have other version installed you might need to update additional files.

### Windows
#### Folder setup
1. Extract downloaded boost source, e.g. `C:\Program Files\boost\boost_1_63_0`.
2. Create a folder for Boost.Build installation, e.g. `C:\Program Files\boost\boost-build`.
3. Create a folder within for building, i.e. `C:\Program Files\boost\boost_1_63_0\build`.
4. Create a folder for installation, e.g. `C:\Program Files\boost\boost`.

#### Boost.Build setup
1. Open Command Prompt and navigate to `C:\Program Files\boost\boost_1_63_0\tools\build`.
2. Open bootstrap.bat in a text editor and find if your Visual Studio toolset version (eg: vc141) is accepted.  
3. Run `bootstrap.bat vc141`. (edit the key name to your toolset version)
4. Run `b2 install --prefix="C:\Program Files\boost\boost-build"`.
5. Add `C:\Program Files\boost\boost-build\bin` to Windows PATH.

#### boost building
1. Navigate to `C:\Program Files\boost\boost_1_63_0`.
2. (Optional) If you don't have `b2.exe` in this folder run `bootstrap.bat vc141` in this location.
3. Run
```
b2 --build-dir="C:\Program Files\boost\boost_1_63_0\build" --prefix="C:\Program Files\boost\boost" toolset=msvc install
```

# Building StarSpace
Note: If building directly from the facebook repo it is necessary to make a bug fix on `model.cpp` under `src`.
*   In line 235 replace `or` by `||`  

The starspace repo in https://github.com/TSPereira/StarSpace.git already has this correction built-in.


### Unix
Compilation is carried out using a Makefile, so you will need to have a working **make**.
Optional: if one wishes to run the unit tests in src directory, google test is required and its path needs to be specified in 'TEST_INCLUDES' in the makefile.

In order to build StarSpace on Mac OS or Linux, use the following commands in terminal:

    git clone https://github.com/TSPereira/StarSpace.git
    cd Starspace
    make

### Windows
Installation on Windows requires Visual Studio

1.  clone `https://github.com/TSPereira/StarSpace.git`
2.  open `MVS\Starspace.sln` in Visual Studio
3.  Right-click on the solution on the Solution Explorer pane on the right and select `Retarget solution`.  
    Choose the `Windows SDK Version` and `Platform toolset` installed on your machine
4.  Update all project properties (one by one):  
    1. Right-click on the project on the right pane and select `Properties`
    2. Under `VC++ Directories`:
        - Add to `Include Directories`: C:\Program Files\boost\boost\include\boost_1_63_0
        - Add to `Library Directories`: C:\Program Files\boost\boost\lib
    
    3. Under `Linker > General` (all projects except StarSpaceLib)
        - Add to `Additional Library Directories`: C:\Program Files\boost\boost\lib
    4. Under `General`, check and if needed correct:
        - `Windows SDK Versions`: should be the one installed on your computer
        - `Platform Toolset`: should be the one installed on your computer and used before
    5. Apply and close
    
5. Optional: if GoogleTest is not installed in the computer unload "matrix_test" and "proj_test"
6. Build the solution

# Python Wrapper
In order to build StarSpace python wrapper, please refer <a href="https://github.com/TSPereira/StarSpace/tree/master/python">README</a> inside the directory <a href="https://github.com/TSPereira/StarSpace/tree/master/python">python</a>.