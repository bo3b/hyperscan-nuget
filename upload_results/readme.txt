hyperscan-nuget

Built on Github using Github Actions.   https://github.com/intel/hyperscan

Builds intel/hyperscan from a git submodule, to create Nuget package.  https://github.com/intel/hyperscan

Developer information for hyperscan from Intel: https://www.intel.com/content/www/us/en/developer/articles/guide/get-started-with-hyperscan-on-windows.html

Based on those instructions, the basic build environment here is Cygwin, 
required for several components. Cmake from VS2019 is required to produce 
a VS2019 sln file, cygwin cmake won't work because it does not have the
"Visual Studio 16 2019" -G target.
msbuild is required for the ending VS2019 build.
The visual studio toolset is set to v141.


This project just packages the Intel/Hyperscan into a nuget package for convenience.
There are no windows binaries or other easy ways to use this package on windows, and
the build process for hyperscan is completely bizarre, so this encapsulates all that into 
a Github Action which generates a simple Nuget download.


Output libs are built for MinSizeRel only, with 4 variants for different platform
and threading models.

	echo	   msbuild the ALL_BUILD.vcxproj
	echo		-t:Rebuild
	echo		-p:Configuration=MinSizeRel
	echo		-p:Platform=x64 then Win32
	echo		-p:PlatformToolset=v141
	echo		_CL_=/MTd then _CL_=/MT

- Output directory structure -

include:
	total 268
	drwxr-xr-x 1 bo3b None     0 Jul 11 18:49 .
	drwxr-xr-x 1 bo3b None     0 Jul 12 03:03 ..
	-rw-r--r-- 1 bo3b None  2441 Jul 11 18:49 allocator.h
	-rw-r--r-- 1 bo3b None  1912 Jul 11 18:49 ch.h
	-rw-r--r-- 1 bo3b None  2394 Jul 11 18:49 ch_alloc.h
	-rw-r--r-- 1 bo3b None 12019 Jul 11 18:49 ch_common.h
	-rw-r--r-- 1 bo3b None 16278 Jul 11 18:49 ch_compile.h
	-rw-r--r-- 1 bo3b None  5741 Jul 11 18:49 ch_database.h
	-rw-r--r-- 1 bo3b None  2109 Jul 11 18:49 ch_internal.h
	-rw-r--r-- 1 bo3b None 12460 Jul 11 18:49 ch_runtime.h
	-rw-r--r-- 1 bo3b None  4358 Jul 11 18:49 ch_scratch.h
	-rw-r--r-- 1 bo3b None  1818 Jul 11 18:49 crc32.h
	-rw-r--r-- 1 bo3b None  3963 Jul 11 18:49 database.h
	-rw-r--r-- 1 bo3b None  7437 Jul 11 18:49 grey.h
	-rw-r--r-- 1 bo3b None  2045 Jul 11 18:49 hs.h
	-rw-r--r-- 1 bo3b None 20813 Jul 11 18:49 hs_common.h
	-rw-r--r-- 1 bo3b None 50824 Jul 11 18:49 hs_compile.h
	-rw-r--r-- 1 bo3b None  3569 Jul 11 18:49 hs_internal.h
	-rw-r--r-- 1 bo3b None 23613 Jul 11 18:49 hs_runtime.h
	-rw-r--r-- 1 bo3b None 14687 Jul 11 18:49 report.h
	-rw-r--r-- 1 bo3b None 10584 Jul 11 18:49 scratch.h
	-rw-r--r-- 1 bo3b None  1810 Jul 11 18:49 scratch_dump.h
	-rw-r--r-- 1 bo3b None  2551 Jul 11 18:49 state.h
	-rw-r--r-- 1 bo3b None  2157 Jul 11 18:49 stream_compress.h
	-rw-r--r-- 1 bo3b None  7620 Jul 11 18:49 stream_compress_impl.h
	-rw-r--r-- 1 bo3b None  7177 Jul 11 18:49 ue2common.h

lib:
	total 20
	drwxr-xr-x 1 bo3b None 0 Jul 12 03:03 .
	drwxr-xr-x 1 bo3b None 0 Jul 12 03:03 ..
	drwxr-xr-x 1 bo3b None 0 Jul 11 15:52 x32_MT
	drwxr-xr-x 1 bo3b None 0 Jul 11 15:46 x32_MTd
	drwxr-xr-x 1 bo3b None 0 Jul 11 16:03 x64_MT
	drwxr-xr-x 1 bo3b None 0 Jul 11 15:58 x64_MTd

lib/x32_MT:
		total 76088
		drwxr-xr-x 1 bo3b None        0 Jul 11 15:52 .
		drwxr-xr-x 1 bo3b None        0 Jul 12 03:03 ..
		-rwxr-xr-x 1 bo3b None    67492 Jul 11 15:51 chimera.lib
		-rwxr-xr-x 1 bo3b None  1335630 Jul 11 15:47 corpusomatic.lib
		-rwxr-xr-x 1 bo3b None   186458 Jul 11 15:46 crosscompileutil.lib
		-rwxr-xr-x 1 bo3b None   219982 Jul 11 15:46 databaseutil.lib
		-rwxr-xr-x 1 bo3b None   383418 Jul 11 15:46 expressionutil.lib
		-rwxr-xr-x 1 bo3b None 55681728 Jul 11 15:51 hs.lib
		-rwxr-xr-x 1 bo3b None  1328972 Jul 11 15:47 hs_runtime.lib
		-rwxr-xr-x 1 bo3b None  3871744 Jul 11 15:51 hsbench.exe
		-rwxr-xr-x 1 bo3b None  2369024 Jul 11 15:51 hscheck.exe
		-rwxr-xr-x 1 bo3b None  3487744 Jul 11 15:52 hscollider.exe
		-rwxr-xr-x 1 bo3b None   278878 Jul 11 15:47 pcre.lib
		-rwxr-xr-x 1 bo3b None  1393486 Jul 11 15:46 sqlite3_static.lib
		-rwxr-xr-x 1 bo3b None  3247616 Jul 11 15:51 unit-chimera.exe
		-rwxr-xr-x 1 bo3b None  4022784 Jul 11 15:52 unit-hyperscan.exe

lib/x32_MTd:
		total 129228
		drwxr-xr-x 1 bo3b None        0 Jul 11 15:46 .
		drwxr-xr-x 1 bo3b None        0 Jul 12 03:03 ..
		-rwxr-xr-x 1 bo3b None    97938 Jul 11 15:46 chimera.lib
		-rwxr-xr-x 1 bo3b None  2211880 Jul 11 15:41 corpusomatic.lib
		-rwxr-xr-x 1 bo3b None   210408 Jul 11 15:41 crosscompileutil.lib
		-rwxr-xr-x 1 bo3b None   250566 Jul 11 15:41 databaseutil.lib
		-rwxr-xr-x 1 bo3b None   510070 Jul 11 15:41 expressionutil.lib
		-rwxr-xr-x 1 bo3b None 92973970 Jul 11 15:46 hs.lib
		-rwxr-xr-x 1 bo3b None  1329020 Jul 11 15:42 hs_runtime.lib
		-rwxr-xr-x 1 bo3b None  7119360 Jul 11 15:46 hsbench.exe
		-rwxr-xr-x 1 bo3b None  5612032 Jul 11 15:46 hscheck.exe
		-rwxr-xr-x 1 bo3b None  6850048 Jul 11 15:46 hscollider.exe
		-rwxr-xr-x 1 bo3b None   278892 Jul 11 15:41 pcre.lib
		-rwxr-xr-x 1 bo3b None  1393486 Jul 11 15:41 sqlite3_static.lib
		-rwxr-xr-x 1 bo3b None  6304768 Jul 11 15:46 unit-chimera.exe
		-rwxr-xr-x 1 bo3b None  7144960 Jul 11 15:46 unit-hyperscan.exe

lib/x64_MT:
		total 116280
		drwxr-xr-x 1 bo3b None        0 Jul 11 16:03 .
		drwxr-xr-x 1 bo3b None        0 Jul 12 03:03 ..
		-rwxr-xr-x 1 bo3b None   124258 Jul 11 16:03 chimera.lib
		-rwxr-xr-x 1 bo3b None  1909554 Jul 11 16:03 corpusomatic.lib
		-rwxr-xr-x 1 bo3b None   255304 Jul 11 16:03 crosscompileutil.lib
		-rwxr-xr-x 1 bo3b None   305636 Jul 11 16:03 databaseutil.lib
		-rwxr-xr-x 1 bo3b None   553852 Jul 11 16:03 expressionutil.lib
		-rwxr-xr-x 1 bo3b None 89394984 Jul 11 16:03 hs.lib
		-rwxr-xr-x 1 bo3b None  1140924 Jul 11 16:03 hs_runtime.lib
		-rwxr-xr-x 1 bo3b None  5069312 Jul 11 16:03 hsbench.exe
		-rwxr-xr-x 1 bo3b None  3597824 Jul 11 16:03 hscheck.exe
		-rwxr-xr-x 1 bo3b None  4615168 Jul 11 16:03 hscollider.exe
		-rwxr-xr-x 1 bo3b None   282552 Jul 11 16:03 pcre.lib
		-rwxr-xr-x 1 bo3b None  1941312 Jul 11 16:03 sqlite3_static.lib
		-rwxr-xr-x 1 bo3b None  4363776 Jul 11 16:03 unit-chimera.exe
		-rwxr-xr-x 1 bo3b None  5486592 Jul 11 16:03 unit-hyperscan.exe

lib/x64_MTd:
		total 199092
		drwxr-xr-x 1 bo3b None         0 Jul 11 15:58 .
		drwxr-xr-x 1 bo3b None         0 Jul 12 03:03 ..
		-rwxr-xr-x 1 bo3b None    158400 Jul 11 15:57 chimera.lib
		-rwxr-xr-x 1 bo3b None   3199260 Jul 11 15:57 corpusomatic.lib
		-rwxr-xr-x 1 bo3b None    277690 Jul 11 15:57 crosscompileutil.lib
		-rwxr-xr-x 1 bo3b None    336256 Jul 11 15:57 databaseutil.lib
		-rwxr-xr-x 1 bo3b None    712182 Jul 11 15:57 expressionutil.lib
		-rwxr-xr-x 1 bo3b None 150069760 Jul 11 15:57 hs.lib
		-rwxr-xr-x 1 bo3b None   1140970 Jul 11 15:57 hs_runtime.lib
		-rwxr-xr-x 1 bo3b None   9660416 Jul 11 15:57 hsbench.exe
		-rwxr-xr-x 1 bo3b None   8186368 Jul 11 15:57 hscheck.exe
		-rwxr-xr-x 1 bo3b None   9373184 Jul 11 15:58 hscollider.exe
		-rwxr-xr-x 1 bo3b None    282576 Jul 11 15:57 pcre.lib
		-rwxr-xr-x 1 bo3b None   1941314 Jul 11 15:57 sqlite3_static.lib
		-rwxr-xr-x 1 bo3b None   8685056 Jul 11 15:57 unit-chimera.exe
		-rwxr-xr-x 1 bo3b None   9815040 Jul 11 15:58 unit-hyperscan.exe
