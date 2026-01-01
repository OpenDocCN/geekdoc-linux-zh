# Windows

You can build curl on Windows in several different ways. We recommend using the MSVC compiler from Microsoft or the free and open source mingw compiler. The build process is, however, not limited to these.

If you use mingw, you might want to use the [autotools](ch043.xhtml#build__autotools__md) build system.

## winbuild

This is how to build curl and libcurl using the command line.

Build with MSVC using the `nmake` utility like this:

[PRE0]

Decide what options to enable/disable in your build. The `README.md` file in that directory details them all, but an example command line could look like this (split into several lines for readability):

[PRE1]

## Visual C++ project files

Using [CMake](ch044.xhtml#build__cmake__md), you can generate a set of Visual Studio project files:

[PRE2]

Once generated, you import them and build with Visual Studio like normally.

## Mingw

You can build curl with the mingw compiler suite. Use [CMake](ch044.xhtml#build__cmake__md) to generate the set of Makefiles for you:

[PRE3]