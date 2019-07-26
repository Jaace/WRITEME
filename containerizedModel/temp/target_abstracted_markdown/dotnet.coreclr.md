# .NET Core Common Language Runtime (CoreCLR)

This repository contains the complete source code for the runtime of @abstr_hyperlink . If you are new to .NET Core start with the @abstr_hyperlink that quickly points you to @abstr_hyperlink .

.NET Core is best thought of as 'agile .NET'. Generally speaking it is the same as the @abstr_hyperlink distributed as part of the Windows operating system, but it is a cross platform (Windows, Linux, macOS) and cross architecture (x @abstr_number , x @abstr_number , ARM) subset that can be deployed as part of the application (if desired), and thus can be updated quickly to fix bugs or add features. 

## If You Just Want to Use .NET Core

Most users don't need to build .NET Core from source since there is already a built and tested version for any supported platform. You can get the latest **released** version of the .NET Core SDK by following the instructions on the @abstr_hyperlink page. If you need the most up to date (daily) version of this .NET Core installer you can get it from the @abstr_hyperlink . If you want one of our official releases, you can get the download from the @abstr_hyperlink . 

## Are you Here for Something Besides the Source Code?

In addition to providing the source code, this repository also acts as a useful nexus for things related to .NET Core including:

  * Want to **learn more** about .NET Runtime Internals? See the Documentation on the .NET Core Runtime page.
  * Need to **log an issue** or provide feedback? See the Issues and Feedback Page page.
  * Want to **chat** with other members of the CoreCLR community? See the Chat Section page.
  * Need a **current build** or **test results** of the CoreCLR repository? See the Official and Daily Builds page.
  * If you want powerful search of the source code for both CoreCLR and CoreFx see @abstr_hyperlink .



## What Can you Make from this Repository?

.NET Core relies heavily on the @abstr_hyperlink package manager, which is a system to package, distribute and version software components. See @abstr_hyperlink for more information on NuGet. For now it is enough to know NuGet is a system that bundles components into `*.nupkg` files (which are ZIP archives) and these packages can be 'published' either through a local file system path or by a URL (e.g. https://www.nuget.org/). There are then tools (e.g. nuget.exe, Visual Studio, dotnet.exe) that based on a configuration file (.csproj) know how to search these publishing locations and pull down consistent set of packages for the application. 

In concrete terms, this repository is best thought of as the source code for the following NuGet package:

  * **Microsoft.NETCore.Runtime.CoreCLR** \- Represents the object allocator, garbage collector (GC), class loader, type system, interop and the most fundamental parts of the .NET class library (e.g. System.Object, System.String ...) 



It also contains the source code for the following closely related support packages. 

  * **Microsoft.NETCore.Jit** \- The Just In Time (JIT) compiler for the @abstr_hyperlink 
  * **Microsoft.NETCore.ILAsm** \- An assembler for the @abstr_hyperlink 
  * **Microsoft.NETCore.ILDAsm** \- A disassembler (Pretty printer) for the @abstr_hyperlink 
  * **Microsoft.NETCore.TestHost** \- This contains the corehost.exe program, which is a small wrapper that uses the .NET Runtime to run IL DLLs passed to it on the command line.
  * **Microsoft.TargetingPack.Private.CoreCLR** \- A set of assemblies that represent the compile time surface area of the class library implemented by the runtime itself.



## Relationship with the @abstr_hyperlink Repository

By itself, the `Microsoft.NETCore.Runtime.CoreCLR` package is actually not enough to do much. One reason for this is that the CoreCLR package tries to minimize the amount of the class library that it implements. Only types that have a strong dependency on the internal workings of the runtime are included (e.g, `System.Object`, `System.String`, `System.Threading.Thread`, `System.Threading.Tasks.Task` and most foundational interfaces). Instead most of the class library is implemented as independent NuGet packages that simply use the .NET Core runtime as a dependency. Many of the most familiar classes (`System.Collections`, `System.IO`, `System.Xml` and so on), live in packages defined in the @abstr_hyperlink repository.

But the main reason you can't do much with CoreCLR is that **ALL** of the types in the class library **LOOK** like they are defined by the CoreFX framework and not CoreCLR. Any library code defined here lives in a single DLL called `System.Private.CoreLib.dll` and as its name suggests is private (hidden). Instead for any particular PUBLIC type defined in CoreCLR, we found the 'right' package in CoreFX where it naturally belongs and use that package as its **public publishing** point. That 'facade' package then forwards references to the (private) implementation in `System.Private.CoreLib.dll` defined here. For example the _`System.Runtime`_ package defined in CoreFX declares the PUBLIC name for types like `System.Object` and `System.String`. Thus from an applications point of view these types live in `System.Runtime.dll`. However, `System.Runtime.dll` (defined in the CoreFX repo) forwards references ultimately to `System.Private.CoreLib.dll` which is defined here.

Thus in order to run an application, you need BOTH the `Microsoft.NETCore.Runtime.CoreCLR` NuGet package (defined in this repository) as well as packages for whatever you actually reference that were defined in the CoreFX repository (which at a minimum includes the `System.Runtime` package). You also need some sort of 'host' executable that loads the CoreCLR package as well as the CoreFX packages and starts your code (typically you use `dotnet.exe` for this). 

These extra pieces are not defined here, however you don't need to build them in order to use the CoreCLR NuGet package you create here. There are already versions of the CoreFX packages published on https://www.nuget.org/ so you can have your test application's project file specify the CoreCLR you built and it will naturally pull anything else it needs from the official location https://www.nuget.org/ to make a complete application. More on this in the Using Your Build page.

* * *

## Setting up your GIT Clone of the CoreCLR Repository

The first step in making a build of the CoreCLR Repository is to clone it locally. If you already know how to do this, just skip this section. Otherwise if you are developing on Windows you can see @abstr_hyperlink for instructions on setting up. This link uses a different repository as an example, but the issues (do you fork or not) and the procedure are equally applicable to this repository. 

* * *

## Building the Repository

The build depends on Git, CMake, Python and of course a C++ compiler. Once these prerequisites are installed the build is simply a matter of invoking the 'build' script (`build.cmd` or `build.sh`) at the base of the repository. 

The details of installing the components differ depending on the operating system. See the following pages based on your OS. There is no cross-building across OS (only for ARM, which is built on X @abstr_number ).   
You have to be on the particular platform to build that platform. 

  * Windows Build Instructions
  * Linux Build Instructions
  * macOS Build Instructions
  * FreeBSD Build Instructions 
  * NetBSD Build Instructions



The build has two main 'buildTypes'

  * Debug (default)- This compiles the runtime with additional runtime checks (asserts). These checks slow runtime execution but are really valuable for debugging, and is recommended for normal development and testing. 
  * Release - This compiles without any development time runtime checks. This is what end users will use but can be difficult to debug. Pass 'release' to the build script to select this. 



In addition, by default the build will not only create the runtime executables, but it will also build all the tests. There are quite a few tests so this does take a significant amount of time that is not necessary if you want to experiment with changes. You can skip building the tests by passing the 'skiptests' argument to the build script.

Thus to get a build as quickly as possible type the following (using `\` as the directory separator, use `/` on Unix machines) @abstr_code_section which will build the Debug flavor which has development time checks (asserts), or @abstr_code_section to build the release (full speed) flavor. You can find more build options with build by using the -? or -help qualifier. 

## Using Your Build

The build places all of its generated files under the `bin` directory at the base of the repository. There is a `bin\Log` directory that contains log files generated during the build (most useful when the build fails). The actual output is placed in a directory like this

  * bin\Product\Windows_NT.x @abstr_number .Release



There are two basic techniques for using your new runtime.

@abstr_number . **Use dotnet.exe and NuGet to compose an application**. See Using Your Build for instructions on creating a program that uses your new runtime by using the 'dotnet' command line interface.

@abstr_number . **Use corerun.exe to run an application using unpackaged Dlls**. This repository also defines a simple host called corerun.exe that does NOT take any dependency on NuGet. Basically it has to be told where to get all the necessary DLLs you actually use, and you have to gather them together 'by hand'. This is the technique that all the tests in the repo use, and is useful for quick local 'edit-compile-debug' loop (e.g. preliminary unit testing). See Using corerun To Run .NET Core Application for details on using this technique. 

## Editing and Debugging

Typically users run through the build and use instructions first with an unmodified build, just to familiarize themselves with the procedures and to confirm that the instructions work. After that you will want to actually make modifications and debug any issues those modifications might cause. See the following links for more. 

  * Editing and Debugging and
  * Documentation on the .NET Core Runtime



## Running Tests

After you have your modification basically working, and want to determine if you have broken anything it is time to run tests. See Running .NET Core Tests for more. 

## Contributing to Repository

Looking for something to work on? The list of @abstr_hyperlink is a great place to start.

Please read the following documents to get started.

  * Contributing Guide
  * Developer Guide



This project has adopted the code of conduct defined by the @abstr_hyperlink to clarify expected behavior in our community. For more information, see the @abstr_hyperlink .

* * *

## Related Projects

As noted above, the CoreCLR Repository does not contain all the source code that makes up the .NET Core distribution. Here is a list of the other repositories that complete the picture. 

  * @abstr_hyperlink - Source for the most common classes in the .NET Framework library.
  * @abstr_hyperlink - Source code for the dotnet.exe program and the policy logic to launch basic .NET Core code (hostfxr, hostpolicy) which allow you to say 'dotnet SOME_CORE_CLR_DLL' to run the app. 
  * @abstr_hyperlink - Source for build time actions supported by dotnet.exe Command line Interface (CLI). Thus this is the code that runs when you do 'dotnet build', 'dotnet restore' or 'dotnet publish'.
  * @abstr_hyperlink - Master copy of documentation for @abstr_hyperlink 



## See Also

  * @abstr_hyperlink is a good place to discover .NET Foundation projects.
  * .NET Core is a @abstr_hyperlink project.
  * @abstr_hyperlink links to @abstr_number s of .NET projects, from Microsoft and the community.
  * The @abstr_hyperlink links to .NET Core related projects from Microsoft.
  * The @abstr_hyperlink is the best place to start learning about ASP.NET Core.



## Important Blog Entries

  * @abstr_hyperlink 
  * @abstr_hyperlink 
  * @abstr_hyperlink 



## License

.NET Core (including the coreclr repo) is licensed under the MIT license.
