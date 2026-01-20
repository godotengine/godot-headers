project('godot-headers',
  'c',
  license : 'MIT',
  meson_version: '>= 1.3.0')

pkgconfig = import('pkgconfig')

h = [
  'android',
  'arvr',
  'gdnative',
  'nativescript',
  'net',
  'pluginscript',
  'videodecoder',
]

install_headers(
  'android/godot_android.h',
  'arvr/godot_arvr.h',
  'gdnative/aabb.h',
  'gdnative/array.h',
  'gdnative/basis.h',
  'gdnative/color.h',
  'gdnative/dictionary.h',
  'gdnative/gdnative.h',
  'gdnative/node_path.h',
  'gdnative/plane.h',
  'gdnative/pool_arrays.h',
  'gdnative/quat.h',
  'gdnative/rect2.h',
  'gdnative/rid.h',
  'gdnative/string.h',
  'gdnative/string_name.h',
  'gdnative/transform.h',
  'gdnative/transform2d.h',
  'gdnative/variant.h',
  'gdnative/vector2.h',
  'gdnative/vector3.h',
  'nativescript/godot_nativescript.h',
  'net/godot_net.h',
  'net/godot_webrtc.h',
  'pluginscript/godot_pluginscript.h',
  'videodecoder/godot_videodecoder.h',
  'gdnative_api_struct.gen.h',
  preserve_path: true)

install_data(
  'api.json',
  'gdnative_api.json',
  'images/faq/create_dlscript.png',
  'images/faq/dllibrary_create_new_dllibrary.png',
  'images/faq/dllibrary_create_new_resource.png',
  'images/faq/dllibrary_save_as_resource.png',
  'images/faq/set_script_dllibrary.png',
  preserve_path: true)

pkgconfig.generate(subdirs: h, name: 'godot-headers', description: 'Headers for the Godot API supplied by the GDNative module')
