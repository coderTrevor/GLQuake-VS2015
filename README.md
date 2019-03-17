# GLQuake-VS2015
Visual Studio 2015 solution for GLQuake by Id software

## Overview
Here's the source code to GLQuake, presented as a Visual Studio 2015 solution. Id's original quake source comes as a Visual C++ 6 workspace, which VS2015 fails to convert. I started with the HandmadeQuake solution made by Philip Buuck. HandmadeQuake only builds winQuake (software rendering). Sadly, handmadequake.org is defunct and Philip removed any videos about the project he posted to his youtube channel.

I removed all the files referencing the software renderer, and added back all the files for the gl renderer which I downloaded from https://github.com/id-Software/Quake

I then proceeded to make minor changes so the project will compile and run without any errors or warnings.


## Instructions
You should be able to load up the solution in Visual Studio 2015 and compile the project without issue. I haven't tested it with Visual Studio 2015 express or newer versions of Visual Studio, but I imagine those versions should work as well.

You will need to copy the Id1 folder from either the shareware or retail versions of the game to the working directory you're launching from. Note that this will be a different folder if you're launching from Visual Studio versus double-clicking on the executable. If you see an error that says something like "W_LoadWadFile: couldn't load gfx.wad" you've done it wrong. I've tested this with the Id1 folder from the Steam version of the game.



## Changes
The compiler thinks "errno" is a function. I changed every instance in the souce to "errorNumber."

The deprecated functions unlink, strnicmp, open, write, and close were replaced with _unlink, _strnicmp, _open, _write, and _close respectively.

Added #include <io.h> to the top of console.c for _open, _write, and _close declarations.

Added #pragma(disable: 4996) to Sys_Init() to disable the error about GetVersionEx() being deprecated.

Enabled some functions in the C code that were probably disabled to use their assembly counterparts. Such functions include BoxOnPlaneSide(), Snd_WriteLinearBlastStereo16(), SND_PaintChannelFrom8(), and SV_HullPointContents().

Disabled 8-bit texture extensions in VID_Init8bitPalette(), because 8-bit textures aren't working.

Disabled printing extensions from GL_Init(). Apparently, modern GPU's contain so many extensions that trying to print them to the console will cause a buffer overflow, crashing the program.

A number of functions were used without being declared. I declared them at the bottom of glquake.h, so compilation can complete without any warnings. There may have been more appropriate headers to add some of these declarations to, but I didn't bother.

Removed x64 as a platform. Compiling for x64 didn't work and I have no interest in fixing it.


## Known Limitations
Project doesn't use any of the assembly code from Id's released source.

8-bit textures don't work at all. I think they might rely on assembly code that isn't being used. I've permanently disabled them in VID_Init8bitPalette(). If you reenable them and don't pass -no8bit to the program from the command line, you'll get to play a cool black-and-white rendition of Quake.

This solution only builds GLQuake. It would have been better to make a seperate project for GLQuake so you could build WinQuake and GLQuake from the same solution, and this is how id organized their original VS6 workspace, but in my eagerness to get GLQuake compiling, I just changed the "HandmadeQuake" project. See Philip Buuck's HandmadeQuake at https://github.com/philipbuuck/Quake-VS2015 if you want to build WinQuake (with software rendering).


## Troubleshooting
If the game exits almost immediately with no error, or if certain textures are missing, try moving the executable and Id1 folder to a directory with a shorter path.
