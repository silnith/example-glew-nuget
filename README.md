# NuGet packaging for GLEW

A simple example of what would be required to create a NuGet package for the GLEW library.

This is a copy of the glew-2.3.1-win32.zip release with two additions.

First, a `nuget` directory is created that contains the files necessary for packaging the
library as a NuGet package suitable for upload to NuGet.org or other artifact repositories.

Second, the `doc/basic.html` file has been translated into Markdown format as `doc/README.md`,
so that the generated NuGet packages can have a README file displayable in package managers.

## Considerations

No attempt is made to alter the existing build process.  It does not even matter
which platform is used to perform the build.  The only thing necessary to create
the NuGet packages is for the distributable files to be placed in a location that
the `nuget.exe` executable can find them.

For simplicity, the GitHub workflow uses a Windows image to run the build, because
those have Visual Studio, MSBuild, and NuGet pre-installed on them.  GitHub has
multiple images available, including ARM images.

Since there is no SDK-style project for the library, using the standard documented
methods for creating NuGet packages will not work.  That is why the `*.nuspec` file
was written by hand, and the package is created directly using `nuget pack ...`.
This can be done without any Visual Studio project at all.

## Static versus DLL

Theoretically it could be possible to create a single NuGet package that can be
controlled using build properties to switch between static and dynamic libraries.
However, this would be confusing for end users, and would require them to hand-edit
the build configurations of any project that wants to make use of GLEW.

Instead, this demonstrates creating separate NuGet packages, one for the static
library and one for the DLL.  This makes it trivial for any end user to find the
library on NuGet.org, add it using the NuGet Package Manager included in Visual
Studio, and build and run their project.  No editing of project files, no special
properties, just add-and-run.
