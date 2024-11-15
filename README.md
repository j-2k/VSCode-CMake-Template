# VSCode-CMake-Template
Visual Studio Code CMake template to get started fast for C++, I will use this for my mac mainly.

#### Requirements
  - C++ Extension in VSC to support IntelliSense & extra stuff
    - You need to have the vscode folder in the root to make sure the intellisense is not giving you errors. For example, iostream was being flagged as not being detected but ofcourse it compiles fine with make, so make sure vscode is auto generated or you have to configure it manually.
  - CMake Tools Extension in VSC
  - Xcode command line tools might also be required if you are using Mac

# Compiling C++ with CMAKE
For compiling, use cmake through the VSC extension or just use cmake CLI, if you dont know how to use the CLI either ask ChatGPT or just use the below reference that I have wrote for another project, im too lazy to edit it right now, it contains information on how to debug with vsc with cmake.

# CPP2PPM Project (Copy Pasted-to be edited later)
Testing how to get a ppm image from cpp, work flow is awful on windows with cmake but whatever.  
I used VS22, and chose a cmake as a base project. Will try to do proper build instructions later, I will try building to both mac & windows.

I'm not sure what workflow I should use but cmake makes me want to use VSC and not VS, but ill just write some stuff for how to make this garbage work, since this took me an hour to figure out.  
to get a image from cpp code in ppm format,  

*this build instructions writing from me is garbage, im going to later copy what other good people write for build instructions, im just writing this for myself*

## Prerequisites
- CMake (version 3.12 or higher)
- A C++ compiler that supports C++20
## Building
### 1. clone the project
```zsh
git clone https://github.com/j-2k/ImagePPM.git
```
### 2. go to the "out" directory
```zsh
#ex > assuming ur in imagePPM master folder, 
cd cpp2ppm_CMake/out/
```
### 3. make an empty build folder in out 
```zsh
#build is already taken & not empty, so build ur
#own proj files with in a different build folder/name
mkdir build2
cd build2
```
### 4. generate the build files
```zsh
#cmake .. goes into the parent directory to find CMAKELISTS.text file
#to generate build files
cmake ..
```
### 5. build the project
```zsh
#do make or cmake --build .
make
#or
cmake --build .
```
### 6. Run the project & output stream to ppm
```zsh
#find the path to the exe (file path/to/directory/with/exe)/jumagfx_cmake.exe > image.ppm
#incase you didnt put the exe in some other place this should just work
./jumagfx_cmake.exe > image.ppm
# the "." will search the current dir & run the exe file remove .exe if on terminal
./jumagfx_cmake > image.ppm
```

## Debug Mode Guide
Getting debug to work on vsc was a slight pain, so im going to document it real quick so I dont ever have to remember how to do it again on vsc.

I will talk about the Cmake Debugger because idk how to use other ones they personally gave me too many issues.  
Firstly, The Cmake Debugger WILL NOT USE THE LAUNCH.json CONFIGURATIONS! It will use settings.json NOT the .vs settings.json but rather the cmakes settings.json file. You can find this file through the cmake extension tools for vscode. See below:  

First navigate to the CMake Tools Extension for VScode then > Set the important garbage up such as:  
1. Build Directory & Args commands if you have any  
2. Cmake Path (command to find cmake path = "whereis cmake")  
3. Go to > Cmake Options Advanced then under it turn on the status bar visibility to visibile!  
![Screenshot 2024-01-10 at 9 42 19 AM](https://github.com/j-2k/ImagePPM/assets/52252068/fe5eb0f1-fd81-4e89-b5c5-f31a1f2f0ce0)
After enabling the status bar to visible you should see the below options, but before continuing, first open the settings.json shown above.
![Screenshot 2024-01-10 at 9 28 17 AM](https://github.com/j-2k/ImagePPM/assets/52252068/92193a2f-eaf2-425b-bc21-874dfcabfe9c)
In the settings.json paste the below to use lldb debug config for mac, other mi modes for other operating systems.
```json
    "cmake.debugConfig": {
        "MIMode": "lldb"
    },
```
Now set the cmake status bar to the correct options like debug & set the build target and etc then run by pressing the bug option on the status bar & it should work!  

![Screenshot 2024-01-10 at 9 52 53 AM](https://github.com/j-2k/ImagePPM/assets/52252068/e3297368-c3a3-4f92-ba38-f69d8542019f)

Its a little weird in the sense sometimes it just skips my breakpoints?? maybe its because i forgot to build but idk it acts weird sometimes.  
But make sure the status bar says debug mode, then build the files set the exe and target then press the bug icon & should run debug mode.  
Thats all!

<details>
  <summary> Old personal build notes</summary>
building is the mostly? the same in all os but im just writing some stuff I learned/did along the way  
maybe just the compiler setting is different between os (msvc) & the build systems(make ninja nmake visualstudio etc)? but idk much about that.
  
## Windows
Build the project with redirecting the standard output stream to a image file (ppm in this case & using CMake, ps I have no idea how to use CMake, so I'm guna learn that before I continue)  
### 1. cmake --build build
build the new changes   
### 2. your/directory/of/executable.exe > image.ppm   
convert output stream to image file ppm, I used cmd, powershell, cmder all cool     
### 3. go to file of executable, image.ppm should be there, open ppm on imagemagic or photoeditor photoshop, gimp etc etc. no native support to open ppm images on windows, however MAC actually lets you open & see PPM images.
## MacOS
Building proj on mac
### 1. enter a empty build folder and run "cmake (PATH TO CMAKELISTS.txt)/." to generate build files
example > in out/build/mac folder is empty, when inside that dir in terminal do "cmake ../../../."
### 2. do "make" or "cmake --build ." to build the project
exmaple > inside mac should be build folders do make or cmake --build . to build project & gen a exe file
### 3. locate jumagfx_cmake.exe and run the command "path/to/jumagfx_cmake.exe > image.ppm" to generate a ppm image in the current directory.
example > "cpp2ppm_CMake/out/build/mac/JumaGFX_CMake/JumaGFX_CMake > image.ppm"
</details>



# C++ notes about file structures with cpp and h files
#### I didnt touch cpp and needed a refresher on translation units & headers so I wrote this here for myself
basic c++ notes about translation units (.cpp) & header files (.h/.hpp),  
use #pragma once to include a header file only ONCE in a translation unit (DONT FORGET TO USE PRAGMA IN H FILES! IMPORTANT!).  
an example of duplicate definitions without pragma:  
assume car.h, engine.h, & main.cpp if main.cpp contains both h files & calls them, with car having to print "car" and engine printing "engine". then main will give "car" & "engine" printed but if the car h file includes engine h without pragma you will get 2 engine prints and 1 car print if that makes sense. So use pragma once in both h files just for sake of guarding against this issue.  
use header files only to hold declarations about certain information, example:  
debug.h > should contain function declarations only related to debugging/console,  
```cpp
//inside debug.h
#pragma once
void Debug(const char* msg);
```
use cpp files to implement the declarations in the header files like so:  
```cpp
//the only line inside debug.cpp
#include "debug.h"
void Debug(const char* msg) {return msg}  
```
in your main to use this function do #include "debug.h" (quotes for files relative to the current file and angled brackets only for compiler include paths)
```cpp
//somewhere in main
#include "debug.h"
#include <iostream>
int main()
{
  Debug("Hello")
}
```

