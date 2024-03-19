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
5. Inside the Awake() class, call PatchAll()
        
        harmony.PatchAll();
6. After the Awake() class, paste the HarmonyPatch class
        
        [HarmonyPatch(typeof(PlayerControllerB), "PlayerJump")]
        class Patch
        {
            static bool Prefix(ref PlayerControllerB __instance)
            {
                // Add patch code here
            }
        }

7. Now, you can start writing code!

To find code to edit, read through the classes in the decompiler (To find game
files, go to Assembly-csharp -> GameNetcodeStuff). Once you find a class you would like 
to edit, replace "PlayerJump" with the name of the class. 

You can now write code in the location indicated. Code written here will act as though 
it was written at the start of the method. You can change variables like so:

    __instance.jumpvalue = 5f;
or return false to prevent a function from being called:

    return false;

### Compilation
Once you are finished writing code, type 

    dotnet build

to compile the code and launch Lethal Company. If your code runs properly, you should see your custom 
message in the terminal and Lethal Company should launch. Remember that the code you write will only 
run clientside.  
