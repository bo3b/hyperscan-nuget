# hyperscan-nuget
Builds Intel/hyperscan to create Nuget package.  https://github.com/intel/hyperscan

Developer information from Intel for hyperscan: https://www.intel.com/content/www/us/en/developer/articles/guide/get-started-with-hyperscan-on-windows.html

<br>

This project just packages the Intel/Hyperscan into a nuget package for convenience.

- The basic build environment here is Cygwin, required for several components.  
- Cmake from VS2019 is required for the "Visual Studio 16 2019" -G target, cygwin cmake won't work.
- msbuild is required for the ending VS2019 build.  
- The visual studio toolset is set to v141.


The build based on Github Action _hyperscan-build.yml_ is setup to use the windows-2019 runner.
That runner includes _only_ VS2019, so we target that version for the CMake -G.  However, 
the msbuild output build uses the v141 toolset as the default installed, not v142.

The _upload_results_ directory is packaged as a nuget nupkg using the included _hyperscan.redist.win.nuspec_
specification.  This is set up to be automatically included as include and lib in a project when the nuget
package is installed.  The _hyperscan.redist.win.targets_ will automatically pick up the proper /MT or /MTd
directories for Debug/Release builds.

----------------

Builds intel/hyperscan from a git submodule@5.4.0, to create Nuget package. 

There are no windows binaries or other easy ways to use this package on windows, and
the build process for hyperscan is completely bizarre, so this encapsulates all that into 
a Github Action which generates a simple Nuget download.

The unit test and util directories for benchmarking are also included, but will be ignored
when imported into Visual Studio.


Output libs are built for MinSizeRel only, with 4 variants for different platform
and threading models.

	echo	   msbuild the ALL_BUILD.vcxproj
	echo		-t:Rebuild
	echo		-p:Configuration=MinSizeRel
	echo		-p:Platform=x64 then Win32
	echo		-p:PlatformToolset=v141
	echo		_CL_=/MTd then _CL_=/MT

Output directory structure -

	include:
	total 268
	drwx------+ 1 bo3b None     0 Jul 13 16:00 .
	drwx------+ 1 bo3b None     0 Jul 14 00:35 ..
	-rwx------+ 1 bo3b None  2441 Jul 13 16:00 allocator.h
	-rwx------+ 1 bo3b None  1912 Jul 13 16:00 ch.h
	-rwx------+ 1 bo3b None  2394 Jul 13 16:00 ch_alloc.h
	-rwx------+ 1 bo3b None 12019 Jul 13 16:00 ch_common.h
	-rwx------+ 1 bo3b None 16278 Jul 13 16:00 ch_compile.h
	-rwx------+ 1 bo3b None  5741 Jul 13 16:00 ch_database.h
	-rwx------+ 1 bo3b None  2109 Jul 13 16:00 ch_internal.h
	-rwx------+ 1 bo3b None 12460 Jul 13 16:00 ch_runtime.h
	-rwx------+ 1 bo3b None  4358 Jul 13 16:00 ch_scratch.h
	-rwx------+ 1 bo3b None  1818 Jul 13 16:00 crc32.h
	-rwx------+ 1 bo3b None  3963 Jul 13 16:00 database.h
	-rwx------+ 1 bo3b None  7437 Jul 13 16:00 grey.h
	-rwx------+ 1 bo3b None  2045 Jul 13 16:00 hs.h
	-rwx------+ 1 bo3b None 20813 Jul 13 16:00 hs_common.h
	-rwx------+ 1 bo3b None 50824 Jul 13 16:00 hs_compile.h
	-rwx------+ 1 bo3b None  3569 Jul 13 16:00 hs_internal.h
	-rwx------+ 1 bo3b None 23613 Jul 13 16:00 hs_runtime.h
	-rwx------+ 1 bo3b None 14687 Jul 13 16:00 report.h
	-rwx------+ 1 bo3b None 10584 Jul 13 16:00 scratch.h
	-rwx------+ 1 bo3b None  1810 Jul 13 16:00 scratch_dump.h
	-rwx------+ 1 bo3b None  2551 Jul 13 16:00 state.h
	-rwx------+ 1 bo3b None  2157 Jul 13 16:00 stream_compress.h
	-rwx------+ 1 bo3b None  7620 Jul 13 16:00 stream_compress_impl.h
	-rwx------+ 1 bo3b None  7177 Jul 13 16:00 ue2common.h

	lib:
	total 20
	drwx------+ 1 bo3b None 0 Jul 13 16:00 .
	drwx------+ 1 bo3b None 0 Jul 14 00:35 ..
	drwx------+ 1 bo3b None 0 Jul 13 16:00 x32_MT
	drwx------+ 1 bo3b None 0 Jul 13 16:00 x32_MTd
	drwx------+ 1 bo3b None 0 Jul 13 16:00 x64_MT
	drwx------+ 1 bo3b None 0 Jul 13 16:00 x64_MTd

		lib/x32_MT:
		total 59472
		drwx------+ 1 bo3b None        0 Jul 13 16:00 .
		drwx------+ 1 bo3b None        0 Jul 13 16:00 ..
		-rwx------+ 1 bo3b None    67452 Jul 13 16:00 chimera.lib
		-rwx------+ 1 bo3b None  1335598 Jul 13 16:00 corpusomatic.lib
		-rwx------+ 1 bo3b None   186450 Jul 13 16:00 crosscompileutil.lib
		-rwx------+ 1 bo3b None   219974 Jul 13 16:00 databaseutil.lib
		-rwx------+ 1 bo3b None   383402 Jul 13 16:00 expressionutil.lib
		-rwx------+ 1 bo3b None 55678440 Jul 13 16:00 hs.lib
		-rwx------+ 1 bo3b None  1328268 Jul 13 16:00 hs_runtime.lib
		-rwx------+ 1 bo3b None   278710 Jul 13 16:00 pcre.lib
		-rwx------+ 1 bo3b None  1393478 Jul 13 16:00 sqlite3_static.lib

		lib/x32_MTd:
		total 96956
		drwx------+ 1 bo3b None        0 Jul 13 16:00 .
		drwx------+ 1 bo3b None        0 Jul 13 16:00 ..
		-rwx------+ 1 bo3b None    97916 Jul 13 16:00 chimera.lib
		-rwx------+ 1 bo3b None  2211906 Jul 13 16:00 corpusomatic.lib
		-rwx------+ 1 bo3b None   210416 Jul 13 16:00 crosscompileutil.lib
		-rwx------+ 1 bo3b None   250570 Jul 13 16:00 databaseutil.lib
		-rwx------+ 1 bo3b None   510090 Jul 13 16:00 expressionutil.lib
		-rwx------+ 1 bo3b None 92973036 Jul 13 16:00 hs.lib
		-rwx------+ 1 bo3b None  1328316 Jul 13 16:00 hs_runtime.lib
		-rwx------+ 1 bo3b None   278724 Jul 13 16:00 pcre.lib
		-rwx------+ 1 bo3b None  1393478 Jul 13 16:00 sqlite3_static.lib

		lib/x64_MT:
		total 93680
		drwx------+ 1 bo3b None        0 Jul 13 16:00 .
		drwx------+ 1 bo3b None        0 Jul 13 16:00 ..
		-rwx------+ 1 bo3b None   124218 Jul 13 16:00 chimera.lib
		-rwx------+ 1 bo3b None  1909522 Jul 13 16:00 corpusomatic.lib
		-rwx------+ 1 bo3b None   255296 Jul 13 16:00 crosscompileutil.lib
		-rwx------+ 1 bo3b None   305628 Jul 13 16:00 databaseutil.lib
		-rwx------+ 1 bo3b None   553836 Jul 13 16:00 expressionutil.lib
		-rwx------+ 1 bo3b None 89391696 Jul 13 16:00 hs.lib
		-rwx------+ 1 bo3b None  1140220 Jul 13 16:00 hs_runtime.lib
		-rwx------+ 1 bo3b None   282384 Jul 13 16:00 pcre.lib
		-rwx------+ 1 bo3b None  1941320 Jul 13 16:00 sqlite3_static.lib

		lib/x64_MTd:
		total 154428
		drwx------+ 1 bo3b None         0 Jul 13 16:00 .
		drwx------+ 1 bo3b None         0 Jul 13 16:00 ..
		-rwx------+ 1 bo3b None    158372 Jul 13 16:00 chimera.lib
		-rwx------+ 1 bo3b None   3199286 Jul 13 16:00 corpusomatic.lib
		-rwx------+ 1 bo3b None    277694 Jul 13 16:00 crosscompileutil.lib
		-rwx------+ 1 bo3b None    336264 Jul 13 16:00 databaseutil.lib
		-rwx------+ 1 bo3b None    712196 Jul 13 16:00 expressionutil.lib
		-rwx------+ 1 bo3b None 150068814 Jul 13 16:01 hs.lib
		-rwx------+ 1 bo3b None   1140266 Jul 13 16:00 hs_runtime.lib
		-rwx------+ 1 bo3b None    282408 Jul 13 16:00 pcre.lib
		-rwx------+ 1 bo3b None   1941322 Jul 13 16:01 sqlite3_static.lib

	unit:
	total 4
	drwx------+ 1 bo3b None 0 Jul 13 16:01 .
	drwx------+ 1 bo3b None 0 Jul 14 00:35 ..
	drwx------+ 1 bo3b None 0 Jul 14 00:35 x32_MT
	drwx------+ 1 bo3b None 0 Jul 14 00:36 x32_MTd
	drwx------+ 1 bo3b None 0 Jul 13 16:01 x64_MT
	drwx------+ 1 bo3b None 0 Jul 13 16:01 x64_MTd

		unit/x32_MT:
		total 3928
		drwx------+ 1 bo3b None       0 Jul 14 00:35 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 4020224 Jul 13 16:01 unit-hyperscan.exe

		unit/x32_MTd:
		total 0
		drwx------+ 1 bo3b None 0 Jul 14 00:36 .
		drwx------+ 1 bo3b None 0 Jul 13 16:01 ..

		unit/x64_MT:
		total 9616
		drwx------+ 1 bo3b None       0 Jul 13 16:01 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 4361728 Jul 13 16:01 unit-chimera.exe
		-rwx------+ 1 bo3b None 5484032 Jul 13 16:01 unit-hyperscan.exe

		unit/x64_MTd:
		total 18048
		drwx------+ 1 bo3b None       0 Jul 13 16:01 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 8674816 Jul 13 16:01 unit-chimera.exe
		-rwx------+ 1 bo3b None 9804800 Jul 13 16:01 unit-hyperscan.exe

	util:
	total 4
	drwx------+ 1 bo3b None 0 Jul 13 16:01 .
	drwx------+ 1 bo3b None 0 Jul 14 00:35 ..
	drwx------+ 1 bo3b None 0 Jul 14 00:35 x32_MT
	drwx------+ 1 bo3b None 0 Jul 14 00:36 x32_MTd
	drwx------+ 1 bo3b None 0 Jul 13 16:01 x64_MT
	drwx------+ 1 bo3b None 0 Jul 13 16:01 x64_MTd

		util/x32_MT:
		total 5716
		drwx------+ 1 bo3b None       0 Jul 14 00:35 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 2367488 Jul 13 16:01 hscheck.exe
		-rwx------+ 1 bo3b None 3485696 Jul 13 16:01 hscollider.exe

		util/x32_MTd:
		total 12160
		drwx------+ 1 bo3b None       0 Jul 14 00:36 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 5606400 Jul 13 16:01 hscheck.exe
		-rwx------+ 1 bo3b None 6843904 Jul 13 16:01 hscollider.exe

		util/x64_MT:
		total 12972
		drwx------+ 1 bo3b None       0 Jul 13 16:01 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 5067776 Jul 13 16:01 hsbench.exe
		-rwx------+ 1 bo3b None 3595264 Jul 13 16:01 hscheck.exe
		-rwx------+ 1 bo3b None 4612608 Jul 13 16:01 hscollider.exe

		util/x64_MTd:
		total 26556
		drwx------+ 1 bo3b None       0 Jul 13 16:01 .
		drwx------+ 1 bo3b None       0 Jul 13 16:01 ..
		-rwx------+ 1 bo3b None 9650176 Jul 13 16:01 hsbench.exe
		-rwx------+ 1 bo3b None 8176128 Jul 13 16:01 hscheck.exe
		-rwx------+ 1 bo3b None 9362944 Jul 13 16:01 hscollider.exe

