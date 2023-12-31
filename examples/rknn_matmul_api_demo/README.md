<!--
 * @Author: Chifred
 * @Date: 2023-04-25 17:32:21
 * @LastEditTime: 2023-04-25 17:47:01
 * @Editors: Chifred
 * @Description: TODO
-->
rknn_matmul_demo is a example which performs int8 matrix multiplication using the RKNPU matmul C API.

Usage:

./rknn_matmul_demo

The following <TARGET_PLATFORM> represents RK356X or RK3588

# Aarch64 Linux Demo
## Build

modify `GCC_COMPILER` on `build-linux_<TARGET_PLATFORM>.sh` for target platform,

then execute

```
./build-linux_<TARGET_PLATFORM>.sh
```

## Install

Copy install/rknn_matmul_demo_Linux to the devices under /userdata/.

- If you use rockchip's evb board, you can use the following way:

Connect device and push the program and rknn model to `/userdata`

```
adb push install/rknn_matmul_demo_Linux /userdata/
```

- If your board has sshd service, you can use scp or other methods to copy the program and rknn model to the board.

## Run

```
adb shell
cd /userdata/rknn_matmul_demo_Linux/
```

```
export LD_LIBRARY_PATH=./lib
./rknn_matmul_demo
```

# Android Demo
## Build

modify `ANDROID_NDK_PATH` on `build-android_<TARGET_PLATFORM>.sh` for target platform, then execute

```
./build-android_<TARGET_PLATFORM>.sh
```

## Install

connect device and push build output into `/data`

```
adb push install/rknn_matmul_demo_Android /data/
```

## Run

```
adb shell
cd /data/rknn_matmul_demo_Android/
```

```
export LD_LIBRARY_PATH=./lib
./rknn_matmul_demo
```
