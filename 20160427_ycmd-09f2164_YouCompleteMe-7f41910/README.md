# Build Instructions

* Download libboost 1.60.0 from https://sourceforge.net/projects/boost/files/boost-binaries/1.60.0/, file : boost_1_60_0-msvc-12.0-32.exe for Visual Studio 2013
* Install / Extract libboost to c:/local/boost_1_60_0
* Download LLVM 3.8 and Install to c:\LLVM
* Genearte Visual Studio Project using cmake
    * Enable clang completer
    * Set LLVM path
* Modify Project Setting for `BoostParts`
    * Linker -> Genernal -> Additional Library Directory, add `c:\local\boost_1_60_0\lib32-msvc-12.0`
    * Linker -> Input -> Additional Dependencies, add 
        * libboost_filesystem-vc120-mt-1_60.lib
        * libboost_system-vc120-mt-1_60.lib
* Modify Project Setting for `BoostParts` and `ycm_core`
	* C/C++ -> Language -> Treat WChar_t as Built in Type, set to Yes
    * Linker -> Genernal -> Additional Library Directory, add `c:\local\boost_1_60_0\lib32-msvc-12.0`
    * Linker -> Input -> Additional Dependencies, add 
        * libboost_python-vc120-mt-1_60.lib
        * libboost_filesystem-vc120-mt-1_60.lib
        * libboost_system-vc120-mt-1_60.lib
        * libboost_regex-vc120-mt-1_60.lib
        * C:\Python27\libs\python27.lib
        * If after those setting, linker report some boost python function redefined, then remove `BoostParts.lib` from linker input and try
* Modify `cpp/BoostParts/libs/thread/src/win32/thread.cpp` to remove line: 
    
     // tss_cleanup_implemented(); // if anyone uses TSS, we need the cleanup linked


## Deps library included as well

