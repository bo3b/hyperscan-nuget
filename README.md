# hyperscan-nuget
Builds intel/hyperscan to create Nuget package.  https://github.com/intel/hyperscan

- The basic build environment here is Cygwin, required for several components.  
- Cmake from VS2019 is required for the "Visual Studio 16 2019" -G target, cygwin cmake won't work.
- msbuild is required for the ending VS2019 build.  
- The visual studio toolset is set to v141.


This project just packages the Intel/Hyperscan into a nuget package for convenience.
There are no windows binaries or other easy ways to use this package on windows, and
the build process for hyperscan is completely bizarre, so this encapsulates all that into 
a Github Action which generates a simple Nuget download.

The build based on Github Action hyperscan-build.yml is setup to use the windows-2019 runner.
That runner includes _only_ VS2019, so we target that version for the CMake -G.  However, 
the msbuild output build uses the v141 toolset as the default installed, not v142.
The upload_results directory is packaged as a nuget nupkg using the included hyperscan.redist.win.nuspec
specification.  This is set up to be automatically included as include/lib in a project when the nuget
package is installed.

----------------

Developer information from Intel: https://www.intel.com/content/www/us/en/developer/articles/guide/get-started-with-hyperscan-on-windows.html

This website is difficult to use, although the information seems mostly correct. 
This repeats the instructions from the website, in an easier to read and use format,
with some comments and tweaks to match this Github build environment.

_Build Environment_  
Hyperscan uses the cross-platform tool CMake to compile, test, and package itself. CMake generates independent configuration files for building on all supported operating systems, for example, UNIX makefiles on Linux* and Microsoft Visual Studio Solution files on Windows. To do a build, Hyperscan uses:

The Ragel state machine compiler to generate the regular expression parser.
The Boost library to create the nondeterministic finite automaton (NFA) graph.
The Perl Compatible Regular Expressions (PCRE) library as a backup to provide complete grammar support for regular expressions.
SQLite to store the corpus.
Python* to record CMake build time.

_Software Requirements_  
Here is the software you'll need, as of the date this article was published. (9/21/18)

- CMake - 2.8.11 or newer  (_We need to use VS2019 CMake_)
- Microsoft Visual Studio 2017 Build Tools (_We will use VS2019 instead_)
- Python - 2.7  (_Not required, used for testing_)
- Ragel - 6.9
- Boost - 1.57 or newer
- PCRE - 8.41
- SQLite - 3.0 or newer

### Step-by-Step Instructions
Install Cygwin  
Download Cygwin, and select make, gcc, and wget as additional installation components. Once the installation is complete, launch the Cygwin terminal, and now you are in the user's home directory.  (_We skip Wget by including components as 7z files._)

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

Add build tools to PATH.
```
$ export VS2019_CMAKE="/cygdrive/c/Program Files (x86)/Microsoft Visual Studio/2019/Enterprise/Common7/IDE/CommonExtensions/Microsoft/CMake/CMake/bin"
$ export VS2019_MSBUILD="/cygdrive/c/Program Files (x86)/Microsoft Visual Studio/2019/Enterprise/MSBuild/Current/Bin"
$ export PATH=$PATH:$VS2019_MSBUILD:$VS2019_CMAKE
```

#### Generate configuration
- Create a new folder named build, then execute CMake commands to generate the configuration.

```
$ cd hyperscan
$ CMake.exe -G "Visual Studio 16 2019" -A Win32 -T v141 -DBOOST_ROOT=boost_1_79_0 -DCMAKE_BUILD_TYPE=MinSizeRel -B build_x32
$ CMake.exe -G "Visual Studio 16 2019" -A x64 -T v141 -DBOOST_ROOT=boost_1_79_0 -DCMAKE_BUILD_TYPE=MinSizeRel -B build_x64
```

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig5-cmake-command-796998.gif)

- Execute CMake commands to build the solution file, or execute MSBuild.exe to build the project file (to use MsBuild.exe, you need to set up the environment variable “PATH” to make sure MSBuild.exe can be found by the system).

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig6-build-solution-file-796998.gif)

```
$ export _CL_=/MTd
$ MSBuild.exe ALL_BUILD.vcxproj -t:Rebuild -p:Configuration=MinSizeRel -p:Platform=Win32 -p:PlatformToolset=v141 -m
$ export _CL_=/MT
$ MSBuild.exe ALL_BUILD.vcxproj -t:Rebuild -p:Configuration=MinSizeRel -p:Platform=Win32 -p:PlatformToolset=v141 -m
$ export _CL_=/MTd
$ MSBuild.exe ALL_BUILD.vcxproj -t:Rebuild -p:Configuration=MinSizeRel -p:Platform=x64 -p:PlatformToolset=v141 -m
$ export _CL_=/MT
$ MSBuild.exe ALL_BUILD.vcxproj -t:Rebuild -p:Configuration=MinSizeRel -p:Platform=x64 -p:PlatformToolset=v141 -m
```

- After compiling, the executables can be found in the _output_libs_ directory.

![image](https://www.intel.com/content/dam/develop/external/us/en/images/webops10610-fig7-confirm-exec-build-796998.gif)

#### Summary
You've successfully installed Hyperscan on Windows. You can use hsbench.exe to check and test your regex. For more information, read about hsbench.exe in the Tools section of the Hyperscan 5.0 Developer's Reference Guide.

#### About the author
Lu Qi is an Intel Software Engineer and a Hyperscan developer.


