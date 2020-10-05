## Development

### Dependencies (Mac)

```sh
brew install glfw3
```

### Dependencies (Linux)

```sh
apt-get install glfw3
```

### Building

```sh
git submodule update --init --recursive


# disable old bundled binding from mapbox-gl-native build
(grep -v "platform/node" ./mapbox-gl-native/platform/macos/macos.cmake > tmp; mv tmp ./mapbox-gl-native/platform/macos/macos.cmake)
(grep -v "platform/node" ./mapbox-gl-native/platform/linux/linux.cmake > tmp; mv tmp ./mapbox-gl-native/platform/linux/linux.cmake)

# install dependencies
npm install
ln -s ../node_modules ./mapbox-gl-native/node_modules
cmake . -B build
cmake --build build
```
