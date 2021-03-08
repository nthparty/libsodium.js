## Compiling a Subset of Sodium Library

```shell
git clone https://github.com/nthparty/libsodium.
cd libsodium.js  # cd cloned repo (js wrappers)
make             # clone libsodium, build libsodium, build wrappers, combine into dist

# 10 minutes later ...

cd libsodium     # cd Sodium submodule (C code)
cd dist-build

# only keep ristretto (and _sodium internals)
perl -pi -e "s/1/0/g" ./emscripten-symbols.def
perl -pi -e "s/0/1/g if /^*ristretto/" ./emscripten-symbols.def
#perl -pi -e "s/0/1/g if /^*_sodium/" ./emscripten-symbols.def

# Comment out subset checking (BUG?) on lines 27-31
vim ./generate-emscripten-symbols.sh

# Generate build symbols
./generate-emscripten-symbols.sh

cd ..  # /libsodium.js/libsodium
# build the Sodium library and JavaScript binding
./dist-build/emscripten.sh --sumo  # or --standard but these are identical now

cd ..  # /libsodium.js
# build wrappers and distribute
make
```
