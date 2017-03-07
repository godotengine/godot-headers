# FAQ

### What is DLScript?
DLScript stands for **Dynamic Linking Script**. It's a module that enables you to use shared libraries, which are dynamically linked, for scripting. DLScript was originally named "cscript" because it exposes a C API, but people thought it was related to C#, which is sometimes abbreviated as "cs". You can build these libraries with C++ as well as C, D, Rust, or any other language that supports C linkage and creating dynamic libraries.

One of the immediate powers to DLScript is the ability to link other shared libraries or to use critical code without recompiling the engine.

Currently there are [C++ bindings](https://github.com/GodotNativeTools/cpp_bindings) that make developing for Godot with C++ easier, but **DLScript is not limited to C++.**

### Will DLScript be official?
**Yes.** DLScript will be official. GDscript would continue to be the primary scripting language, and progress on integrating other languages into Godot will continue.

### Can you use one library for all scripts?
**Yes.** You may use one library for all scripts. Remember the name field that is always greyed out for GDScript? You need to set a name for the script here.

### Can you debug with DLScript?
**Yes.** You must compile the library with debug symbols, and then you can use your debugger as usual.

### Can I use GDScript in the same project?
**Yes.** You may use DLScript and GDScript in the same project.

### How can I add my own bindings for DLScript?
**More details will be available soon.**

### What are the requirements?
Linux implementation is available as of right now.

[You must build Godot with the DLScript module.](https://github.com/bojidar-bg/godot/tree/dlscript-module)

### How do I compile my library?
First, create a function with C linkage ```void godot_dlscript_init(godot_dlscript_init_options*)```, which will be the entry point for Godot.

Use ```godot_script_*``` methods **only** in the ```godot_dlscript_init()``` function.

Then, compile your code into a dynamic library. Example for C:
-   ```clang -c -g -std=c11 -fPIC test.c -I/path/to/godot/headers/ -o test.os```
-   ```clang -g -shared test.os -o test.so```

```-g``` is for debugging information

### How do I use DLScript from the editor?

Manually create a **DLLibrary** resource. Save it as a **.tres**. This resource contains links to the libraries for each platform.

![](images/faq/create_dllibrary_resource.png?raw=true)

Now, **create a new DLScript on your node.** You may prefer built-in script for DLScript, unless you want to organize many **.dl** files which only contain a name. **You must specify the name of the class you would like to use.**

![](images/faq/create_dlscript.png?raw=true)

DLScript resources have a field for a DLLibrary. **This field is empty by default.**

![](images/faq/set_script_dllibrary.png?raw=true)

If you leave it empty, **you can set a default DLLibrary in the project settings** ```dlscript/default_dllibrary```.

![](images/faq/set_project_dllibrary.png?raw=true)
