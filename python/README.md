# Starwrap

This is the python wrapper for Starspace. Which is not complete. Below are the APIs those are done and to be done.
#### done:
```
init
initFromTsv
initFromSavedModel
train
evaluate
getDocVector
nearestNeighbor
saveModel
saveModelTsv
loadBaseDocs
predictTags
```
#### to be done:
```
getNgramVector
printDoc
predictOne
```

## How to build?
- make sure you have CMake installed. Otherwise install it from [here](https://cmake.org/install/)
- install Conan c++ package manager from [here](https://conan.io/downloads.html).

Note: If building directly from the facebook repo it is necessary to make a bug fix on `CMakeLists.txt`.
*   Replace line 98/99 for:  


    set_property(TARGET starwrap APPEND_STRING PROPERTY COMPILE_FLAGS "/Os /GL ")
    set_property(TARGET starwrap APPEND_STRING PROPERTY LINK_FLAGS "/LTCG ")
    
The starspace repo in https://github.com/TSPereira/StarSpace.git already has this correction built-in.

### Unix
1.  clone this repository
2.  move to the directory `StarSpace > python`.
3.  run the build script. 

    ```
    chmod +x build.sh
    ./build.sh
    ```

    build script will download necessary packages, build wrapper and run test code.  
4.  When completed, you will find `starwrap.so` inside newly created `build` directory.   
5.  Either copy `starwrap.so` into your python project or set `LD_LIBRARY_PATH` (GNU/Linux) `DYLD_LIBRARY_PATH` (Mac OSX) and import is as a python module `import starwrap`.

### Windows
1.  Confirm (and if needed add) that both CMake and Conan are on your windows path
2.  Navigate to `Starspace/python`
3.  Create folders `build` and `lib`
4.  Copy `Starspace/MVS/x64/Debug/StarSpaceLib.lib` to the new `lib` folder (to check if needed)
5.  Open Command Prompt with Administrative Rights:
    - Navigate to `Starspace/python/build`
    - run `conan install ..`
    - run `cmake .. -DCMAKE_BUILD_TYPE=Release`
6.  Open `python\build\starspace.sln` in Visual Studio
7.  On the top bar dropdown boxes, open the 2nd and open `Configuration Manager...`
    - Press on the `Active Solution platform` dropdown box and choose `New`.
    - Select `x64` and copy from `Win32`
8.  Open `starwrap` project properties:
    1.  Right-click on the project on the right pane and select `Properties`
    2.  Under `VC++ Directories` add to `Library Directories`: 
        - Starspace\MVS\x64\Debug
        - C:\Program Files\boost\boost\lib (as per example of boost installation used in Starspace build instructions)
    3.  (Optional) If installing Starspace in a specific python environment, under `C/C++ > General` change in `Additional Include Directories`:
        - `anaconda3\include` folder to `<env_dir>\include`
    4.  Under `Linker > All Options > Additional Dependencies` add:
        - StarSpaceLib.lib
        - starspace.lib
        - <env_dir>\libs\<wanted_python_version>.lib
    5.  Under `Linker > All Options > Additional Options`: delete everything
9.  Build the Solution
10. Make the resulting `starwrap.pyd` available to the interpreter by using one of the follow:
    - Copy `starwrap.pyd` from `build\x64\Debug` folder to your project folder
    - add that folder as an external library path in the python interpreter
    - add a `<some_name>.pth` file with the path to the folder with the `starwrap.pyd` file in `site-packages` folder


### Python Version
Python wrapper will be looking for python 3.6. If you need to use a different version of python do the following
 change (example for python 3.7 in environment <env>):

#### UNIX
Go to `python/build.sh` and replace as follows
    
    -cmake .. -DCMAKE_BUILD_TYPE=Release
    +cmake .. -DCMAKE_BUILD_TYPE=Release -DPYTHON_LIBRARY=/<path to anaconda>/envs/<env>/lib/libpython3.7m.dylib -DPYTHON_INCLUDE_DIR=/<path to anaconda>/envs/<env>/include/python3.7m/
    
#### Windows
When in the Command Prompt, after the command 

    conan install ..
    
run
    
    cmake .. -DCMAKE_BUILD_TYPE=Release -DPYTHON_LIBRARY=/<path to anaconda>/envs/<env>/python3.7.dll -DPYTHON_INCLUDE_DIR=/<path to anaconda>/envs/<env>/include/


## How to use?
API is very easy and straightforward. Please refer `test.py` in `test` directory.

### How to predict tags?
If you train a model in trainMode=0, you can get tag predictions. Here is the Python code to get prediction for `barack obama` in the political_social_media dataset.

```
sp.initFromSavedModel('tagged_model')
sp.initFromTsv('tagged_model.tsv')

dict_obj = sp.predictTags('barack obama', 10)
dict_obj = sorted( dict_obj.items(), key = itemgetter(1), reverse = True )

for tag, prob in dict_obj:
    print( tag, prob )
```

And you get this:
```
__label__obama 0.5291043519973755
__label__stopobamasamnesty 0.5073596239089966
__label__florida 0.5003609657287598
__label__entitlement 0.47724902629852295
__label__savesarah 0.4583539664745331
__label__keystone 0.4561977982521057
__label__1866 0.43746984004974365
__label__waroncoal 0.4283019006252289
__label__notleading 0.4117577075958252
__label__cuba 0.3887191414833069
```
For the full example, please refer to `test_predictTags.py` in `test` directory.
