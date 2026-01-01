# BoringSSL

## build boringssl

$HOME/src is where I put the code in this example. You can pick wherever you like.

[PRE0]

## set up the build tree to get detected by curl’s configure

In the boringssl source tree root, make sure there is a `lib` and an `include` dir. The `lib` directory should contain the two libs (I made them symlinks into the build dir). The `include` directory is already present by default. Make and populate `lib` like this (commands issued in the source tree root, not in the `build/` subdirectory).

[PRE1]

## configure curl

`LIBS=-lpthread ./configure --with-ssl=$HOME/src/boringssl` (where I point out the root of the boringssl tree)

Verify that at the end of the configuration, it says it detected BoringSSL to be used.

## build curl

Run `make` in the curl source tree.

Now you can install curl normally with `make install` etc.