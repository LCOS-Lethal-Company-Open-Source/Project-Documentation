# Creating a lethal company mod
These are the instructions for making a Lethal Company mod with our template. 

### Software installation 
Before creating any mods, you'll need to install the proper sofware for modding. Instructions 
can be found [here](https://lethal.wiki/dev/initial-setup). 

Install the .Net SDK (we will be using this to build our code) and install 
Visual Studio Code if you do not have it already. We will also need a decompiler 
to write code; in this case, we will be using dnSpyEx. It is also recommended that 
you install Unity. 

If you need instructions on how to use dnSpy, you can find them [here](https://lethal.wiki/dev/fundamentals/reading-game-code).

### Using the LCOS template
LCOS has made a template for any LCOS modders to use, which includes contributor credits, 
a logo, and a code template. To properly utilize the template:
1. Download the files from Github, and copy them into your local branch.
2. Change the name of the Template.csproj file to a name you choose (henceforth referred to as [yourname]). 
3. Inside Plugin.cs, change the text "namespace Template;" to "namespace [yourname]".
4. Create a new harmony object inside the BaseUnityPlugin class
        
        Harmony harmony = new Harmony(PluginInfo.PLUGIN_GUID);
5. Inside the Awake() function, call PatchAll(). In Unity, the Awake function is called when an object is initialized, this code is treated as a Unity object, so any code in Awake() will be called on initialization. The PatchAll() function will patch any HarmonyPatch to the game's code. 
        
        harmony.PatchAll();
    
    A HarmonyPatch is essentially a way of modifying or hijacking a game's source code. There are three types of patches we will be uses to make our mods: Prefix, Postfix, and Transpiler. Each of these patches is applied to a function that is called by the game. 

    For more information on harmony and how to use it, you can access harmonylib's documentation [here](https://harmony.pardeike.net/articles/intro.html).

6. To start creating a patch, write the following code underneath (not inside) the Awake function:
        
        [HarmonyPatch(typeof([class_name]), "[function_name]")]
        class [Patch_Name]
        {
      
        }
        

    [class_name] will be the name of the class that contains the function we want to modify, and [function_name] will be the name of the function. Make sure that the function name is a string. [Patch_Name] can be whatever name you want, but please make it descriptive of what the patch is actually doing. Inside this class will be the function which actually patches the game's code. 

    ### Prefix

    When the function being patched is called, a prefix will run its code before the function is ran (hence why it's called a prefix). 

    Here is an example of a Prefix patch:

        static bool Prefix(ref [class_name] __instance)
        {
            // Add patch code here
        }
    
    ref [class_name] __instance gets the instance of the class which is calling this function, which can be very useful.

    A prefix can also control if the function is run at all. If a prefix returns false, any prefix after it and the function itself will not be run. Make sure to return true if you want to function to run after your prefix!

    For more information on prefix, see the harmonylib documentation on prefix [here](https://harmony.pardeike.net/articles/patching-prefix.html).

    ### Postfix

    A postfix is very similar to a prefix, except instead of running before the function it runs after the function. 

    Here is an example of a Postfix patch:

        static void Postfix(ref [type] __result)
        {
            // Add patch code here
        }

    In this case, [type] is the return type of the function which we are patching. ref [type] __result will get the result of the function which is being called before the postfix (this can also be the result of a prefix patch which skipped the function). 

    For more information on prefix, see the harmonylib documentation for postfix [here](https://harmony.pardeike.net/articles/patching-postfix.html).

    ### Transpiler

    The tranpiler allows you to directly* modify the code of the function itself. Here is the shell a tranpiler function:

        static IEnumerable<CodeInstruction> Transpiler (IEnumerable<CodeInstruction> instructions)
        {
            foreach (CodeInstruction instruction in instructions)
            {
                // Patch code
                yield return instruction;
            }
        }

    I know, I know, it looks very scary, just work with me here... 

    Let's start with what the function is taking in. The instructions are essentially the list of code instructions that are being called by the function (what the function is doing). These code instructions are NOT C#, rather they are CIL (common intermediate language). CIL is the middle point between C# and the low-level assembly code which is being run by the CPU. In order to edit this code, you will need to have a basic understanding of CIL code. You can find a list of all CIL code instructions [here](https://en.wikipedia.org/wiki/List_of_CIL_instructions). 

    Now let us move on to what the function is actually doing...

    Basically, the function is going through each of the original instructions of the function, doing (something), and then returning the instruction that is actually wanted (either the original instruction or a new one). The function returns, one at a time, the new list of instructions that the function will run. 

     For more information on the transpiler, see the harmonylib documentation for transpiler [here](https://harmony.pardeike.net/articles/patching-transpiler.html).
 
7. Now, you can start writing code! 

    To find code to edit, read through the classes in the decompiler (To find game
    files, go to Assembly-csharp -> GameNetcodeStuff)
`
### Compilation
Once you are finished writing code, type 

    dotnet build

to compile the code and launch Lethal Company. If your code runs properly, you should see your custom 
message in the terminal and Lethal Company should launch. Remember that the code you write will only 
run clientside.  
