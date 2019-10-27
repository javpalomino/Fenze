# Copyright 2018 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
licenses(["notice"])  # zlib, portions BSD, MIT, PostgreSQL
# Common include paths
sdl_includes = [
    "include",
    "src/video/khronos",
]
objc_library(
    name = "sdl2_objc",
    srcs = glob([
        "src/**/*.h",
        "include/*.h",
    ]),
    includes = sdl_includes,
    non_arc_srcs = glob([
        "src/audio/coreaudio/*.m",
        "src/file/cocoa/*.m",
        "src/filesystem/cocoa/*.m",
        "src/render/metal/*.m",
        "src/video/cocoa/*.m",
    ]),
    sdk_frameworks = [
        "AudioToolbox",
        "Carbon",
        "CoreAudio",
        "CoreVideo",
        "Cocoa",
        "ForceFeedback",
        "IOKit",
        "OpenGL",
        "Metal",
    ],
    alwayslink = 1,
)
sdl_srcs = glob(
    include = [
        "src/**/*.c",
        "src/**/*.h",
    ],
    exclude = [
        "src/video/qnx/**",
        "src/haptic/windows/**",
        "src/test/*.c",
        "src/thread/generic/*.c",
        "src/core/linux/*.c",
    ],
)
# In general, we bundle SDL2.  However, on Linux that would require a header
# generated by --configure, and a lot of buildflags and installing dev headers
# is in general less painful, so we just link it dynamically.
cc_library(
    name = "sdl2",
    srcs = select({
        "@javicho_code//buildenv:linux": [],
        "@javicho_code//buildenv:osx": sdl_srcs,
    }),
    hdrs = glob(["include/*.h"]),
    includes = sdl_includes,
    linkopts = select({
        "@javicho_code//buildenv:linux": ["-lSDL2"],
        "@javicho_code//buildenv:osx": [],
    }),
    textual_hdrs = glob(["src/thread/generic/*.c"]),
    visibility = ["//visibility:public"],
    deps = select({
        "@javicho_code//buildenv:linux": [],
        "@javicho_code//buildenv:osx": [":sdl2_objc"],
    }),
)