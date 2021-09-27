# godot-headers

This repository contains C headers for
[**Godot Engine**](https://github.com/godotengine/godot)'s *GDNative Extensions* API.

> `GDNative Extensions` enables the use of dynamically linked libraries inside of
> [**Godot**](https://github.com/godotengine/godot).

- [**Versioning**](#versioning)
- [**Using this library**](#using-this-library)
- [**Updating Headers**](#updating-headers)

## Versioning

This repositories follows the same branch versioning as the main [Godot Engine
repository](https://github.com/godotengine/godot):

- `master` tracks the current development branch. As this is a moving target,
  the headers in this repository may not always be fully in sync with upstream.
  See [**Updating Headers**](#updating-headers) if you need to bring
  them up to date.
- `3.x` tracks the development of the next 3.x minor release. Like `master`, it
  might not always be fully up-to-date with upstream.
- Other versioned branches (e.g. `3.3`, `3.2`) track the latest stable release
  in the corresponding branch.

Stable releases are also tagged on this repository:
[**Tags**](https://github.com/godotengine/godot-headers/tags).

**For any project built against a stable release of Godot, we recommend using
this repository as a Git submodule, checking out the specific tag matching your
Godot version.**

## Using this library

This library is meant to be used in conjunction with a code generator that creates the binding code necesairy to interact with Godot. This library only provides access to the API over which communication with the Godot core is implemented and binding meta data in the form of a json document.

One such binding library is [godot-cpp](https://github.com/godotengine/godot-cpp) which implements the bindings using C++.

A pure C sample project is currently not available.

## Updating Headers

See [**Versioning**](#versioning) for details on the Godot versions tracked by
each branch of this repository.

If the relevant branch is not up-to-date for your needs, or if you want to sync
the headers with your own modified version of Godot, here is the update
procedure used to sync this repository with upstream releases:

- Compile [Godot Engine](https://github.com/godotengine/godot) at the specific
  version/commit which you are using.
- Use the compiled executable to generate the `extension_api.json` file with:
  `godot --dump-extension-api extension_api.json`
- Copy the file `core/extension/gdnative_interface.h` to `godot`
