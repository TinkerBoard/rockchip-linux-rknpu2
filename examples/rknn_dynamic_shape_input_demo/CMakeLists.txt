cmake_minimum_required(VERSION 3.4.1)

project(rknn_dynshape_demo)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# rknn api
if(TARGET_SOC STREQUAL "rk356x")
  set(RKNN_API_PATH ${CMAKE_SOURCE_DIR}/../../runtime/RK356X/${CMAKE_SYSTEM_NAME}/librknn_api)
elseif(TARGET_SOC STREQUAL "rk3588")
  set(RKNN_API_PATH ${CMAKE_SOURCE_DIR}/../../runtime/RK3588/${CMAKE_SYSTEM_NAME}/librknn_api)
else()
  message(FATAL_ERROR "TARGET_SOC is not set, ref value: rk356x or rk3588")
endif()

if (CMAKE_SYSTEM_NAME STREQUAL "Android")
  set(RKNN_RT_LIB ${RKNN_API_PATH}/${CMAKE_ANDROID_ARCH_ABI}/librknnrt.so)
else()
  if (CMAKE_C_COMPILER MATCHES "aarch64")
    set(LIB_ARCH aarch64)
  else()
    set(LIB_ARCH armhf)
  endif()
  set(RKNN_RT_LIB ${RKNN_API_PATH}/${LIB_ARCH}/librknnrt.so)
endif()
include_directories(${RKNN_API_PATH}/include)
include_directories(${CMAKE_SOURCE_DIR}/../3rdparty)

# opencv
if (CMAKE_SYSTEM_NAME STREQUAL "Android")
    set(OpenCV_DIR ${CMAKE_SOURCE_DIR}/../3rdparty/opencv/OpenCV-android-sdk/sdk/native/jni/abi-${CMAKE_ANDROID_ARCH_ABI})
else()
  if(LIB_ARCH STREQUAL "armhf")
    set(OpenCV_DIR ${CMAKE_SOURCE_DIR}/../3rdparty/opencv/opencv-linux-armhf/share/OpenCV)
  else()
    set(OpenCV_DIR ${CMAKE_SOURCE_DIR}/../3rdparty/opencv/opencv-linux-aarch64/share/OpenCV)
  endif()
endif()
find_package(OpenCV REQUIRED)

#### cnpy
set(CNPY_ROOT ${CMAKE_SOURCE_DIR}/../3rdparty/cnpy)
include_directories(${CNPY_ROOT})

set(CMAKE_INSTALL_RPATH "lib")

### 普通API Demo
add_executable(rknn_dynshape_inference
    src/rknn_dynshape_inference.cc
    ${CNPY_ROOT}/cnpy.cpp
)
target_link_libraries(rknn_dynshape_inference
  ${RKNN_RT_LIB}
  ${OpenCV_LIBS}
)
# install target and libraries
set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/install/rknn_dynshape_demo_${CMAKE_SYSTEM_NAME})
install(TARGETS rknn_dynshape_inference DESTINATION ./)

## 零拷贝API Demo
add_executable(rknn_dynshape_inference_zero_copy
    src/rknn_dynshape_inference_zero_copy.cc
    ${CNPY_ROOT}/cnpy.cpp
)
target_link_libraries(rknn_dynshape_inference_zero_copy
  ${RKNN_RT_LIB}
  ${OpenCV_LIBS}
)
# install target and libraries
set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/install/rknn_dynshape_demo_${CMAKE_SYSTEM_NAME})
install(TARGETS rknn_dynshape_inference_zero_copy DESTINATION ./)

install(DIRECTORY model DESTINATION ./)
install(DIRECTORY images DESTINATION ./)
install(PROGRAMS ${RKNN_RT_LIB} DESTINATION lib)
