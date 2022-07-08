# hyperscan-nuget
Builds intel/hyperscan to create Nuget package.  https://github.com/intel/hyperscan

- The basic build environment here is Cygwin, required for several components.  
- Cmake from VS2019 is required to produce a VS2019 sln file, cygwin cmake won't work.
- msbuild is required for the ending VS2019 build.  


Developer information from Intel: https://www.intel.com/content/www/us/en/developer/articles/guide/get-started-with-hyperscan-on-windows.html

This website is horrible, although the information is mostly correct.  It has some twisted boxes inside boxes UI that makes it really difficult to follow the instructions.  Instead of that idiotic nested thing, let's repeat the info here, fixing errors and doing it sensibly, inline. 

----------------

_Build Environment_  
Hyperscan uses the cross-platform tool CMake to compile, test, and package itself. CMake generates independent configuration files for building on all supported operating systems, for example, UNIX makefiles on Linux* and Microsoft Visual Studio Solution files on Windows. To do a build, Hyperscan uses:

The Ragel state machine compiler to generate the regular expression parser.
The Boost library to create the nondeterministic finite automaton (NFA) graph.
The Perl Compatible Regular Expressions (PCRE) library as a backup to provide complete grammar support for regular expressions.
SQLite to store the corpus.
Python* to record CMake build time.

_Software Requirements_  
Here is the software you'll need, as of the date this article was published. (9/21/18)

- CMake - 2.8.11 or newer  (We need to use VS2019 CMake)
- Microsoft Visual Studio 2017 Build Tools (Will use VS2019 instead)
- Python - 2.7  (Not required, used for testing)
- Ragel - 6.9
- Boost - 1.57 or newer
- PCRE - 8.41
- SQLite - 3.0 or newer

### Step-by-Step Instructions
Install Cygwin  
Download Cygwin, and select make, gcc, and wget as additional installation components. Once the installation is complete, launch the Cygwin terminal, and now you are in the user's home directory.

#### Download and build Ragel  
Type the following commands to download and compile Ragel.

```
$ wget  http://www.colm.net/files/ragel/ragel-6.10.tar.gz
$ tar xzvf ragel-6.10.tar.gz && rm ragel-6.10.tar.gz
$ cd ragel-6.10
$./configure
$ make
$ make install
```
![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig1-install-compile-ragel-796998.gif)

#### Download build tools
- Download CMake Windows Edition and Python.   
- Download Microsoft Visual Studio Build Tools 2017 (you may download the whole IDE, which comes with Build Tools; but if you only want to compile Hyperscan at the command line, downloading the Build Tools will be enough.)

#### Prepare Hyperscan
First, download Hyperscan.  (Already done as submodule)

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig3-download-hyperscan-796998.gif)

#### Get dependent libraries
get the dependent libraries: Boost, PCRE, and SQLite.

- Download Boost (version 1.57 and above), PCRE (version 8.41 and above), as well as SQLite-amalgamation zip.
- Unzip downloaded files to the hyperscan folder
- Rename the SQLite-amalgamation to SQLite3  

The final directory structure will look like this:
![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig4-install-boost-pcre-sqlite-0-796998.gif)


#### Generate configuration
- Create a new folder named build, then execute CMake commands to generate the configuration.

```
$ cd hyperscan
$ mkdir build
$ CMake -G "Visual Studio 16 2019" -D BOOST_ROOT=../boost_1_79_0/..
```

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig5-cmake-command-796998.gif)

- Execute CMake commands to build the solution file, or execute MSBuild.exe to build the project file (to use MsBuild.exe, you need to set up the environment variable “PATH” to make sure MSBuild.exe can be found by the system).

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig6-build-solution-file-796998.gif)

```
$ export PATH=$PATH:"/cygdrive/c/Program Files (x86)/Microsoft Visual Studio/2017/BuildTools/MSBuild/15.0/Bin/"
$ MsBuild.exe ALL_BUILD.vcxproj /t:build
```

- After compiling, the executables can be found in the bin directory.

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig7-confirm-exec-build-796998.gif)

#### Summary
You've successfully installed Hyperscan on Windows. You can use hsbench.exe to check and test your regex. For more information, read about hsbench.exe in the Tools section of the Hyperscan 5.0 Developer's Reference Guide.

#### About the author
Lu Qi is an Intel Software Engineer and a Hyperscan developer.


