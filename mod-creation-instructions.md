# Creating a lethal company mod
These are the instructions for making a Lethal Company mod with our template. 

1. Software installation 
Before creating any mods, you'll need to install the proper sofware for modding. Instructions 
can be found [here](https://lethal.wiki/dev/initial-setup). 

Install the .Net SDK (we will be using this to build our code) and install 
Visual Studio Code if you do not have it already. We will also need a decompiler 
to write code; in this case, we will be using dnSpyEx. It is also recommended that 
you install Unity. 

If you need instructions on how to use dnSpy, you can find them [here](https://lethal.wiki/dev/fundamentals/reading-game-code).

2. Using the LCOS template
LCOS has made a template for any LCOS modders to use, which includes contributor credits, 
a logo, and a code template. To properly utilize the template:
- Download the files from Github, and copy them into your local branch.
- Change the name of the Template.csproj file to a name you choose (henceforth referred to as [yourname]). 
- Inside Plugin.cs, change the text "namespace Template;" to "namespace [yourname]".