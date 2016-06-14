# Build YouCompleteMe On Windows

## Introduction
This is guide for build `YouCompleteMe` server `ycmd` on Windows system for 32-bit vim. You can just get the revision specified below and copy my binary file into your `YouCompleteMe\third_party\ycmd` folder

### Revision Information
`ycmd` : 0825f85
`YouCompleteMe` : dec2287

## Build Guide

First, you need to follow the official guide @ `https://github.com/Valloric/YouCompleteMe/wiki/Windows-Installation-Guide` to generate `Visual Studio` project file using `CMake`. 

### Change Source File Under BoostParts Project

To avoid build error of `BoostParts`, add following code block at the end of file `ycmd\cpp\BoostParts\libs\thread\src\win32\tss_pe.cpp`. But this step can be avoid because I will use the full `Boost` library instead of `BoostParts`. Details describe in the following sections.

```
namespace boost
{
	void tss_cleanup_implemented()
	{
		/*
		This function's sole purpose is to cause a link error in cases where
		automatic tss cleanup is not implemented by Boost.Threads as a
		reminder that user code is responsible for calling the necessary
		functions at the appropriate times (and for implementing an a
		tss_cleanup_implemented() function to eliminate the linker's
		missing symbol error).

		If Boost.Threads later implements automatic tss cleanup in cases
		where it currently doesn't (which is the plan), the duplicate
		symbol error will warn the user that their custom solution is no
		longer needed and can be removed.
		*/
	}
}

```

### Modify Compiler and Linker Options

To avoid linker error `error LNK2019: unresolved external symbol "void __cdecl boost::filesystem::path_traits::convert`

For project `ycm_client_support` and `ycm_core`, right click, choose `Property`, then

* Add `/Zc:wchar_t ` in `C/C++ -> Command Line -> Additional Options`
* Do Same for `Linker -> Command Line -> Additional Options`

### Change Linker Additional Library

#### Download Full Boost Library
`BoostParts` library looks like not complete for compile on Windows system. Use `BoostParts` will see many linker errors relate to `boost::system` and `boost::filesystem` such as:

```
error LNK2001: unresolved external symbol "class boost::system::error_category const & __cdecl boost::system::system_category(void)

error LNK2019: unresolved external symbol "class boost::filesystem::path __cdecl boost::filesystem::detail::current_path(class boost::system::error_code *)

error LNK2019: unresolved external symbol "class boost::filesystem::path __cdecl boost::filesystem::detail::temp_directory_path(class boost::system::error_code *)
```

So I change to official Boost library. Get the full library in binary format from `http://sourceforge.net/projects/boost/files/boost-binaries/`. I downloaded `boost_1_58_0-msvc-12.0-32.exe` because I will use `Visual Studio 2013 Express for Desktop` to compile. Then install to `C:\local`. If you install to other directory, please change the library path accordingly below. 

#### Change Settings

For project `ycm_client_support` and `ycm_core`, right click, choose `Property`, then edit `Linker -> Input -> Additional Dependencies` 

Remove Line:

```
..\..\BoostParts\Release\BoostParts.lib
```

Add following lines:

```
C:\Python27\libs\python27.lib
C:\local\boost_1_58_0\lib32-msvc-12.0\libboost_system-vc120-mt-1_58.lib
C:\local\boost_1_58_0\lib32-msvc-12.0\libboost_filesystem-vc120-mt-1_58.lib
C:\local\boost_1_58_0\lib32-msvc-12.0\libboost_python-vc120-mt-1_58.lib
C:\local\boost_1_58_0\lib32-msvc-12.0\libboost_regex-vc120-mt-1_58.lib
C:\local\boost_1_58_0\lib32-msvc-12.0\boost_system-vc120-mt-1_58.lib
C:\local\boost_1_58_0\lib32-msvc-12.0\boost_filesystem-vc120-mt-1_58.lib
```
