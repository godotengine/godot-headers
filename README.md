# FAQ

### What is GDNative?
GDNative is a module that enables you to use shared libraries, which are dynamically linked, for scripting. GDNative was originally named "cscript" because it exposes a C API, but people thought it was related to C#, which is sometimes abbreviated as "cs". Then it was named "DLScript" but that brought up some confusion, so we settled with GDNative. You can build these libraries with C++ as well as C, D, Rust, or any other language that supports C linkage and creating dynamic libraries.

One of the immediate powers to native scripts is the ability to link other shared libraries or to use critical code without recompiling the engine.

Currently there are [C++ bindings](https://github.com/GodotNativeTools/cpp_bindings) that make developing for Godot with C++ easier, but **native scripts are not limited to C++.**

### Are native scripts on the offical builds?
**Yes.**

### Can you use one library for all scripts?
**Yes.** You may use one library for all scripts. Remember the name field that is always greyed out for GDScript? You need to set a name for the script here.

### Can you debug native scripts?
**Yes.** You must compile the library with debug symbols, and then you can use your debugger as usual.

### Can I use GDScript in the same project?
**Yes.** You may use native scripts and GDScripts in the same project.

### How can I add my own bindings for native scripts?
**More details will be available soon.**

### What are the requirements?
Linux implementation is tested. OSX and Android *should* work as well. Windows is implemented but not tested yet. 

[You must build a recent version of Godot.](https://github.com/godotengine/godot)

### Creating a GDNative library
Getting `godot_headers`
```
$ cd SimpleLibrary
$ git clone https://github.com/GodotNativeTools/godot_headers
```
right now our file structure should look like this
```
[SimpleLibrary]
	├── godot_headers/
	├── lib/
	└── src/
```

# Creating our test Library / Script

Create `test.c` under `SimpleLibrary/src/` and add the following code
```
#include <godot/gdnative.h>
#include <godot_nativescript.h>

#include <stdio.h>

void *test_constructor(godot_object *obj, void *method_data) {
        printf("test.constructor()\n");
        return 0;
}

void test_destructor(godot_object *obj, void *method_data, void *user_data) {
        printf("test.destructor()\n");
}

/** func _ready() **/
godot_variant test_ready(godot_object *obj, void *method_data, void *user_data, int num_args, godot_variant **args) {
        godot_variant ret;
        godot_variant_new_nil(&ret);

        printf("_ready()\n");

        return ret;
}

/** Library entry point **/
void GDN_EXPORT godot_gdnative_init(godot_gdnative_init_options *o) {
}

/** Library de-initialization **/
void GDN_EXPORT godot_gdnative_terminate(godot_gdnative_terminate_options *o) {
}

/** Script entry (Registering all the classes and stuff) **/
void GDN_EXPORT godot_nativescript_init(void *desc) {
	printf("nativescript init\n");

	godot_instance_create_func create_func = {
		.create_func = &test_constructor,
                .method_data = 0,
                .free_func   = 0
        };

        godot_instance_destroy_func destroy_func = {
                .destroy_func = &test_destructor,
                .method_data  = 0,
                .free_func    = 0
        };

        godot_nativescript_register_class(desc, "SimpleClass", "Node", create_func, destroy_func);

        {
                godot_instance_method method = {
                        .method = &test_ready,
                        .method_data = 0,
                        .free_func = 0
                };

                godot_method_attributes attr = {
                        .rpc_type = GODOT_METHOD_RPC_MODE_DISABLED
                };

                godot_nativescript_register_method(desc, "SimpleClass", "_ready", attr, method);
        }
}

godot_variant GDN_EXPORT some_test_procedure(void *data, godot_array *args) {
        godot_variant ret;
        godot_variant_new_int(&ret, 42);

        godot_string s;
        godot_string_new_unicode_data(&s, L"Hello World", 11);
        godot_print(&s);

        godot_string_destroy(&s);

        return ret;
}

```

### How do I compile my library?
To compile your code into a dynamic library:
```
$ clang -g -fPIC -std=c99 -c src/test.c -I/path/to/godot/headers/ -o src/test.os
$ clang -g -shared src/test.os -o lib/test.so
```

Note:
	`-g` is for debugging information
	Use `godot_nativescript_*` methods **only** in the `nativescript_init()` function.

### How do I use native scripts from the editor?

Manually create a **GDNativeLibrary** resource.
![](images/faq/dllibrary_create_new_resource.png)

![](images/faq/dllibrary_create_new_dllibrary.png)

Save it as a **.tres**.

![](images/faq/dllibrary_save_as_resource.png)

This resource contains links to the libraries for each platform.

### Using the library in GDScript
```
extends Node

func _ready():
        var gdn = GDNative.new()
        gdn.library = load("res://lib/libtest.tres")

        gdn.initialize()

        var res = gdn.call_native("standard_varcall", "some_test_procedure", [])

        print("result: ", res)

        gdn.terminate()
```

### Attaching GDNative library to your `Node`

Now, **create a new native script on your node.** You may prefer built-in script for native scripts, unless you want to organize many **.gdns** files which only contain a name. **You must specify the name of the class you would like to use.**

![](images/faq/create_dlscript.png?raw=true)

"NativeScript" resources have a field for a GDNativeLibrary. **This field is empty by default.**

![](images/faq/set_script_dllibrary.png?raw=true)


