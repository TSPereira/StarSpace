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
- now, clone this repository, move to the directory `StarSpace > python`.
- run the build script. 

```
chmod +x build.sh
./build.sh
```

- build script will download necessory packages, build wrapper and run test code. 
- when it is done, you will find `starwrap.so` inside newly created `build` directory. 
- you can either copy `starwrap.so` into your python project or set `LD_LIBRARY_PATH` (GNU/Linux) `DYLD_LIBRARY_PATH` (Mac OSX) and import is as a python module `import starwrap`.

To build on Windows follow the next steps:

    Install CMake (to check if needed) (don't forget to add cmake to windows path)
	Install Conan c++ package manager (don't forget to add conan to windows path)
	Navigate to Starspace/python
	
    The next lines will replicate the "build.sh" which is prepared for UNIX only
	- create folders "build" and "lib"
	- Copy "Starspace/MVS/x64/Debug/StarSpaceLib.lib" to the new "lib" folder (to check if needed)
	- Open Command Prompt with Administrative Rights:
		- Navigate to "build"
		- run "conan install .."
		- run "cmake .. -DCMAKE_BUILD_TYPE=Release" (to check if needed)
	- Open starspace.sln in Visual Studio
	- On the top bar dropdown boxes, open the 2nd and open "Configuration Manager..."4
		- Press on the "Active Solution platform" dropdown box and choose "New".
		- Select "x64" and copy from "Win32"
	- Open "starwrap" project properties:
		- Under Configuration Properties>VC++ Directories add to Library Directories: 
			- Starspace\MVS\x64\Debug
			- <boost_dir>\<boost_version_dir>\stage\lib
		- Under C/C++ > General change in Additional Include Directories (if using different env):
			- "include" folder to "<env_dir>\include"
		- Under Linker > All Options:
			- Additional Dependencies add:
				- StarSpaceLib.lib
				- starspace.lib
				- <env_dir>\libs\<wanted_python_version>.lib
			- Additional Options: delete everything
		- Under Linker > Command Line:
			- Delete everything from Additional Options
	- Build the Solution
	- Copy the resulting "starwrap.pyd" in the "build\x64\Debug" folder to your project folder or add that folder as an external library path in the python interpreter

Python wrapper will be looking for python 3.6. If you need to use a different version of python do the following
 change (example for python 3.7 in environment <env>):

- UNIX

    Go to python/build.sh and replace as follows

    ```
    -cmake .. -DCMAKE_BUILD_TYPE=Release
    +cmake .. -DCMAKE_BUILD_TYPE=Release -DPYTHON_LIBRARY=/<path to anaconda>/envs/<env>/lib/libpython3.7m.dylib
   -DPYTHON_INCLUDE_DIR=/<path to anaconda>/envs/<env>/include/python3.7m/
    ```
 
- Windows

    When in the Command Prompt, after the command
    
    ```
    conan install ..
    ```  
    
    run
    
    ```
    cmake .. -DCMAKE_BUILD_TYPE=Release -DPYTHON_LIBRARY=/<path to anaconda>/envs/<env>/python3.7.dll
   -DPYTHON_INCLUDE_DIR=/<path to anaconda>/envs/<env>/include/
    ```
  
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
