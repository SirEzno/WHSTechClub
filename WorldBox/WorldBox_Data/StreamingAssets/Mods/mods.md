=== Drop Mod .dll files here ===

The compiled dll file should be a 64bit library.

## Entry point in your mod:

Every mod needs to have a `WorldBoxMod` class that inherits from `MonoBehaviour`. This class will be loaded when your mod is loaded. From here you can instantiate other classes and run your update cycles, etc.

You can see all the available methods and their execution order in this handy chart : https://docs.unity3d.com/Manual/ExecutionOrder.html

An example `my_mod.cs` :

```
namespace my_mod 
{
    using UnityEngine;
    public class WorldBoxMod : MonoBehaviour
    {
        // https://docs.unity3d.com/Manual/ExecutionOrder.html

        // called every time this gameobject is enabled
        // and before everything else
        public void Awake() {
            UnityEngine.Debug.Log("my_mod called awake");
        }

        // Runs every time we enable the mod
        void OnEnable() {
            // setup your mod here
        }

        // Runs every time we disable the mod
        void OnDisable() {
            // clean up nicely here, please
        }

        // Can be used for initialization on first load
        // only ever called once
        void Start () {
            UnityEngine.Debug.Log("my_mod called start");
        }
        
        // Update is called once per frame
        void Update () {
            if(!Config.gameLoaded) return;
        }
    }
}
```

## Building a mod
- You can build it with a variety of tools, an example command could look like :

```
/Applications/Unity/Hub/Editor/2019.2.11f1/Unity.app/Contents/MonoBleedingEdge/bin/mcs \
    -r:/path_to_worldbox/worldbox.app/Contents/Resources/Data/Managed/UnityEngine.CoreModule.dll \
    -r:/path_to_worldbox/worldbox.app/Contents/Resources/Data/Managed/UnityEngine.dll \
    -r:/path_to_worldbox/worldbox.app/Contents/Resources/Data/Managed/Assembly-CSharp.dll \
    -target:library \
    -sdk:4 \
    -platform:x64 \
    -out:my_mod.dll \
    my_mod.cs
```

## Naming a mod
- Important: Every mod needs its' unique name, namespace and assembly name.
             So if your namespace is called "great_mod", then the dll has to be compiled to "great_mod.dll" as well.
             It is not enough to rename a dll - as the dll has the original name reference inside it.