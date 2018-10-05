# Build the image
## android-arm64
> docker build --rm -t choiip/xcompile-android-arm64 android-arm64

## android-sdk
> docker build --rm -t choiip/xcompile-android-sdk android-sdk

# Run the containers
## SDK
### copy the pre-downloaded SDK to the mounted 'sdk' directory
> docker run -it --rm -v $(pwd)/sdk:/sdk choiip/xcompile-android-sdk bash -c 'cp -a $ANDROID_HOME/. /sdk'

### update SDK in the host
> sdk/tools/bin/sdkmanager --update

### mount without read-only option, this allows to update SDK inside container
> docker run -it -v $(pwd)/sdk:/opt/android-sdk choiip/xcompile-android-sdk /bin/bash


## NDK 
### build the script (through dockcross)
> docker run --rm dockcross/android-arm64 > ./bin/xcompile-android-arm64 && chmod +x ./bin/xcompile-android-arm64

### build the script (through xcompile)
> docker run --rm choiip/xcompile-android-arm64 > ./bin/xcompile-android-arm64 && chmod +x ./bin/xcompile-android-arm64

### set PATH environment variable
> PATH=$PWD/bin:$PATH

### run the script
> xcompile-android-arm64 /bin/bash

### run the script with ndk mounted
> xcompile-android-arm64 -a "-v $(pwd)/android-ndk-r16b:/opt/android-ndk:ro" /bin/bash

### run the script with both sdk and ndk mounted
> xcompile-android-arm64 -a "-v $(pwd)/sdk:/opt/android-sdk -v $(pwd)/android-ndk-r16b:/opt/android-ndk:ro" -i choiip/xcompile-android-arm64 /bin/bash
