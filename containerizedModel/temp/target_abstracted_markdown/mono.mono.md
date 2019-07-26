Mono is a software platform designed to allow developers to easily create cross platform applications. It is an open source implementation of Microsoft's .NET Framework based on the ECMA standards for C# and the Common Language Runtime.

The Mono project is part of the @abstr_hyperlink 

@abstr_hyperlink 

@abstr_number . Compilation and Installation @abstr_number . Using Mono @abstr_number . Directory Roadmap @abstr_number . Contributing to Mono @abstr_number . Reporting bugs @abstr_number . Configuration Options @abstr_number . Working with Submodules

### Build Status

| OS | Architecture | Status | |--------------|--------------------|------------------------------| | Debian @abstr_number | amd @abstr_number | @abstr_hyperlink | | Debian @abstr_number | i @abstr_number | @abstr_hyperlink | | Debian @abstr_number | armel | @abstr_hyperlink | | Debian @abstr_number | armhf | @abstr_hyperlink | | Debian @abstr_number | arm @abstr_number | @abstr_hyperlink | | OS X | amd @abstr_number | @abstr_hyperlink | | OS X | i @abstr_number | @abstr_hyperlink | | Windows | amd @abstr_number | @abstr_hyperlink | | Windows | i @abstr_number | @abstr_hyperlink | | CentOS | s @abstr_number x (cs) | @abstr_hyperlink | | Debian @abstr_number | ppc @abstr_number el (cs) | @abstr_hyperlink | | AIX @abstr_number . @abstr_number | ppc @abstr_number (cs) | @abstr_hyperlink | | FreeBSD @abstr_number | amd @abstr_number (cs) | @abstr_hyperlink |

_(cs) = community supported architecture_

# Compilation and Installation

## Building the Software

Please see our guides for building Mono on @abstr_hyperlink , @abstr_hyperlink and @abstr_hyperlink .

Note that building from Git assumes that you already have Mono installed, so please download and @abstr_hyperlink before trying to build from Git. This is required because the Mono build relies on a working Mono C# compiler to compile itself (also known as @abstr_hyperlink ).

## If you don't have a working Mono installation

If you don't have a working Mono installation, you can try a slightly more risky approach: getting the latest version of the 'monolite' distribution, which contains just enough to run the 'mcs' compiler. You do this with:
    
    
    # Run the following line after ./autogen.sh
    make get-monolite-latest
    

This will download and place the files appropriately so that you can then just run:
    
    
    make
    

The build will then use the files downloaded by `make get-monolite-latest`.

## Testing and Installation

You can run the mono and mcs test suites with the command: `make check`.

Expect to find a few test suite failures. As a sanity check, you can compare the failures you got with @abstr_hyperlink .

You can now install mono with: `make install`

You can verify your installation by using the mono-test-install script, it can diagnose some common problems with Mono's install. Failure to follow these steps may result in a broken installation. 

# Using Mono

Once you have installed the software, you can run a few programs:

  * `mono program.exe` runtime engine

  * `mcs program.cs` C# compiler

  * `monodis program.exe` CIL Disassembler




See the man pages for mono( @abstr_number ), mcs( @abstr_number ) and monodis( @abstr_number ) for further details.

# Directory Roadmap

  * `acceptance-tests/` \- Optional third party test suites used to validate Mono against a wider range of test cases.

  * `data/` \- Configuration files installed as part of the Mono runtime.

  * `docs/` \- Technical documents about the Mono runtime.

  * `external/` \- Git submodules for external libraries (Newtonsoft.Json, ikvm, etc).

  * `ikvm-native/` \- Glue code for ikvm.

  * `libgc/` \- The (deprecated) Boehm GC implementation.

  * `llvm/` \- Utility Makefiles for integrating the Mono LLVM fork.

  * `m @abstr_number /` \- General utility Makefiles.

  * `man/` \- Manual pages for the various Mono commands and programs.

  * `mcs/` \- The class libraries, compiler and tools

    * `class/` \- The class libraries (like System.*, Microsoft.Build, etc.)

    * `mcs/` \- The Mono C# compiler written in C#

    * `tools/` \- Tools like gacutil, ikdasm, mdoc, etc.

  * `mono/` \- The core of the Mono Runtime.

    * `arch/` \- Architecture specific portions.

    * `benchmark/` \- A collection of benchmarks.

    * `btls/` \- Build files for the BTLS library which incorporates BoringSSL.

    * `cil/` \- Common Intermediate Representation, XML definition of the CIL bytecodes.

    * `dis/` \- CIL executable Disassembler.

    * `eglib/` \- Independent implementation of the glib API.

    * `metadata/` \- The object system and metadata reader.

    * `mini/` \- The Just in Time Compiler.

    * `profiler/` \- The profiler implementation.

    * `sgen/` \- The SGen Garbage Collector implementation.

    * `tests/` \- The main runtime tests.

    * `unit-tests/` \- Additional runtime unit tests.

    * `utils/` \- Utility functions used across the runtime codebase.

  * `msvc/` \- Logic for the MSVC / Visual Studio based runtime and BCL build system. The latter is experimental at the moment.

  * `packaging/` \- Packaging logic for the OS X and Windows Mono packages.

  * `po/` \- Translation files.

  * `runtime/` \- A directory that contains the Makefiles that link the mono/ and mcs/ build systems.

  * `samples/` \- Some simple sample programs on uses of the Mono runtime as an embedded library.

  * `scripts/` \- Scripts used to invoke Mono and the corresponding program.

  * `sdks/` \- A new way of embedding Mono into Xamarin.iOS, Xamarin.Android and other products.

  * `support/` \- Various support libraries.

  * `tools/` \- A collection of tools, mostly used during Mono development.




# Contributing to Mono

Before submitting changes to Mono, please review the @abstr_hyperlink . Please pay particular attention to the @abstr_hyperlink section.

# Reporting bugs

To submit bug reports, please @abstr_hyperlink .

Please use the search facility to ensure the same bug hasn't already been submitted and follow our @abstr_hyperlink on how to make a good bug report.

# Configuration Options

The following are the configuration options that someone building Mono might want to use:

  * `--with-sgen=yes,no` \- Generational GC support: Used to enable or disable the compilation of a Mono runtime with the SGen garbage collector.

    * On platforms that support it, after building Mono, you will have both a `mono-boehm` binary and a `mono-sgen` binary. `mono-boehm` uses Boehm, while `mono-sgen` uses the Simple Generational GC.
  * `--with-libgc=[included, none]` \- Selects the default Boehm garbage collector engine to use.

    * _included_ : ( _slightly modified Boehm GC_ ) This is the default value for the Boehm GC, and it's the most feature complete, it will allow Mono to use typed allocations and support the debugger.

    * _none_ : Disables the inclusion of a Boehm garbage collector.

    * This defaults to `included`.

  * `--enable-cooperative-suspend`

    * If you pass this flag the Mono runtime is configured to only use the cooperative mode of the garbage collector. If you do not pass this flag, then you can control at runtime the use of the cooperative GC mode by setting the `MONO_ENABLE_COOP_SUSPEND` flag.
  * `--with-tls=__thread,pthread`

    * Controls how Mono should access thread local storage, pthread forces Mono to use the pthread APIs, while __thread uses compiler-optimized access to it.

    * Although __thread is faster, it requires support from the compiler, kernel and libc. Old Linux systems do not support with __thread.

    * This value is typically pre-configured and there is no need to set it, unless you are trying to debug a problem.

  * `--with-sigaltstack=yes,no`

    * **Experimental** : Use at your own risk, it is known to cause problems with garbage collection and is hard to reproduce those bugs.

    * This controls whether Mono will install a special signal handler to handle stack overflows. If set to `yes`, it will turn stack overflows into the StackOverflowException. Otherwise when a stack overflow happens, your program will receive a segmentation fault.

    * The configure script will try to detect if your operating system supports this. Some older Linux systems do not support this feature, or you might want to override the auto-detection.

  * `--with-static_mono=yes,no`

    * This controls whether `mono` should link against a static library (libmono.a) or a shared library (libmono.so). 

    * This defaults to `yes`, and will improve the performance of the `mono` program. 

    * This only affects the `mono' binary, the shared library libmono.so will always be produced for developers that want to embed the runtime in their application.

  * `--with-xen-opt=yes,no` \- Optimize code for Xen virtualization.

    * It makes Mono generate code which might be slightly slower on average systems, but the resulting executable will run faster under the Xen virtualization system.

    * This defaults to `yes`.

  * `--with-large-heap=yes,no` \- Enable support for GC heaps larger than @abstr_number GB.

    * This only applies only to the Boehm garbage collector, the SGen garbage collector does not use this configuration option.

    * This defaults to `no`.

  * `--enable-small-config=yes,no` \- Enable some tweaks to reduce memory usage and disk footprint at the expense of some capabilities.

    * Typically this means that the number of threads that can be created is limited ( @abstr_number ), that the maximum heap size is also reduced ( @abstr_number MB) and other such limitations that still make mono useful, but more suitable to embedded devices (like mobile phones).

    * This defaults to `no`.

  * `--with-ikvm-native=yes,no` \- Controls whether the IKVM JNI interface library is built or not.

    * This is used if you are planning on using the IKVM Java Virtual machine with Mono.

    * This defaults to `yes`.

  * `--with-profile @abstr_number =yes,no` \- Whether you want to build the @abstr_number .x profile libraries and runtime.

    * This defaults to `yes`.
  * `--with-libgdiplus=installed,sibling,<path>` \- Configure where Mono searches for libgdiplus when running System.Drawing tests.

    * It defaults to `installed`, which means that the library is available to Mono through the regular system setup.

    * `sibling` can be used to specify that a libgdiplus that resides as a sibling of this directory (mono) should be used.

    * Or you can specify a path to a libgdiplus.

  * `--enable-minimal=LIST`

    * Use this feature to specify optional runtime components that you might not want to include. This is only useful for developers embedding Mono that require a subset of Mono functionality.
    * The list is a comma-separated list of components that should be removed, these are:

    * `aot`: Disables support for the Ahead of Time compilation.

    * `attach`: Support for the Mono.Management assembly and the VMAttach API (allowing code to be injected into a target VM)

    * `com`: Disables COM support.

    * `debug`: Drop debugging support.

    * `decimal`: Disables support for System.Decimal.

    * `full_messages`: By default Mono comes with a full table of messages for error codes. This feature turns off uncommon error messages and reduces the runtime size.

    * `generics`: Generics support. Disabling this will not allow Mono to run any @abstr_number . @abstr_number libraries or code that contains generics.

    * `jit`: Removes the JIT engine from the build, this reduces the executable size, and requires that all code executed by the virtual machine be compiled with Full AOT before execution.

    * `large_code`: Disables support for large assemblies.

    * `logging`: Disables support for debug logging.

    * `pinvoke`: Support for Platform Invocation services, disabling this will drop support for any libraries using DllImport.

    * `portability`: Removes support for MONO_IOMAP, the environment variables for simplifying porting applications that are case-insensitive and that mix the Unix and Windows path separators.

    * `profiler`: Disables support for the default profiler.

    * `reflection_emit`: Drop System.Reflection.Emit support

    * `reflection_emit_save`: Drop support for saving dynamically created assemblies (AssemblyBuilderAccess.Save) in System.Reflection.Emit.

    * `shadow_copy`: Disables support for AppDomain's shadow copies (you can disable this if you do not plan on using appdomains).

    * `simd`: Disables support for the Mono.SIMD intrinsics library.

    * `ssa`: Disables compilation for the SSA optimization framework, and the various SSA-based optimizations.

  * `--enable-llvm`

  * `--enable-loadedllvm`

    * This enables the use of LLVM as a code generation engine for Mono. The LLVM code generator and optimizer will be used instead of Mono's built-in code generator for both Just in Time and Ahead of Time compilations.

    * See https://www.mono-project.com/docs/advanced/mono-llvm/ for the full details and up-to-date information on this feature.

    * You will need to have an LLVM built that Mono can link against.

    * The `--enable-loadedllvm` variant will make the LLVM backend into a runtime-loadable module instead of linking it directly into the main mono binary.

  * `--enable-big-arrays` \- Enable use of arrays with indexes larger than Int @abstr_number .MaxValue.

    * By default Mono has the same limitation as .NET on Win @abstr_number and Win @abstr_number and limits array indexes to @abstr_number -bit values (even on @abstr_number -bit systems).

    * In certain scenarios where large arrays are required, you can pass this flag and Mono will be built to support @abstr_number -bit arrays.

    * This is not the default as it breaks the C embedding ABI that we have exposed through the Mono development cycle.

  * `--enable-parallel-mark`

    * Use this option to enable the garbage collector to use multiple CPUs to do its work. This helps performance on multi-CPU machines as the work is divided across CPUS.

    * This option is not currently the default on OSX as it runs into issues there.

    * This option only applies to the Boehm GC.

  * `--enable-dtrace`

    * On Solaris and MacOS X builds a version of the Mono runtime that contains DTrace probes and can participate in the system profiling using DTrace.
  * `--disable-dev-random`

    * Mono uses /dev/random to obtain good random data for any source that requires random numbers. If your system does not support this, you might want to disable it.

    * There are a number of runtime options to control this also, see the man page.

  * `--with-csc=roslyn,mcs,default`

    * Use this option to configure which C# compiler to use. By default the configure script will pick Roslyn, except on platforms where Roslyn does not work (Big Endian systems) where it will pick mcs.

If you specify "mcs", then Mono's C# compiler will be used. This also allows for a complete bootstrap of Mono's core compiler and core libraries from source.

If you specify "roslyn", then Roslyn's C# compiler will be used. This currently uses Roslyn binaries.

  * `--enable-nacl`

    * This configures the Mono compiler to generate code suitable to be used by Google's Native Client: https://code.google.com/p/nativeclient/

    * Currently this is used with Mono's AOT engine as Native Client does not support JIT engines yet.

  * `--enable-wasm`

    * Use this option to configure mono to run on WebAssembly. It will set both host and target to the WebAssembly triplet. This overrides the values passed to `--host` or `--target` and ignored what config.sub guesses.

This is a workaround to enable usage of old automake versions that don't recognize the wasm triplet.




# Working With Submodules

Mono references several external git submodules, for example a fork of Microsoft's reference source code that has been altered to be suitable for use with the Mono runtime.

This section describes how to use it.

An initial clone should be done recursively so all submodules will also be cloned in a single pass:
    
    
    $ git clone --recursive git@github.com:mono/mono
    

Once cloned, submodules can be updated to pull down the latest changes. This can also be done after an initial non-recursive clone:
    
    
    $ git submodule update --init --recursive
    

To pull external changes into a submodule:
    
    
    $ cd <submodule>
    $ git pull origin <branch>
    $ cd <top-level>
    $ git add <submodule>
    $ git commit
    

By default, submodules are detached because they point to a specific commit. Use `git checkout` to move back to a branch before making changes:
    
    
    $ cd <submodule>
    $ git checkout <branch>
    # work as normal; the submodule is a normal repo
    $ git commit/push new changes to the repo (submodule)
    
    $ cd <top-level>
    $ git add <submodule> # this will record the new commits to the submodule
    $ git commit
    

To switch the repo of a submodule (this should not be a common or normal thing to do at all), first edit `.gitmodules` to point to the new location, then:
    
    
    $ git submodule sync -- <path of the submodule>
    $ git submodule update --recursive
    $ git checkout <desired new hash or branch>
    

The desired output diff is a change in `.gitmodules` to reflect the change in the remote URL, and a change in / where you see the desired change in the commit hash.

# License

See the LICENSE file for licensing information, and the PATENTS.TXT file for information about Microsoft's patent grant.

# Mono Trademark Use Policy

The use of trademarks and logos for Mono can be found @abstr_hyperlink . 

# Maintaining the Class Library Solution Files

Mono now ships with a solution file that can be used to build the assemblies from an IDE. Either by opening the topmost `net_ @abstr_number _x.sln` file, or to by loading one of the individual `csproj` files located in each directory.

These are maintained by extracting the configuration information from our Makefiles, which as of May @abstr_number remain the canonical location for configuration information.

When changes are made to the Makefiles, a user would need to run the following command to re-generate the solution files at the top level:
    
    
    $ make update-solution-files
    
