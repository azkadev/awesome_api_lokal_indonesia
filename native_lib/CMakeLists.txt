cmake_minimum_required(VERSION 3.16)

project(awesome_api_lokal_indonesia_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE PACKAGE_TEMPLATE")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(awesome_api_lokal_indonesia_shared_dir "lib")
set(awesome_api_lokal_indonesia_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "awesome_api_lokal_indonesia_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "awesome_api_lokal_indonesia")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "awesome_api_lokal_indonesia")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "awesome_api_lokal_indonesia")
endif()


add_library(awesome_api_lokal_indonesia_shared SHARED awesome_api_lokal_indonesia.cpp)
add_executable(awesome_api_lokal_indonesia_exe awesome_api_lokal_indonesia.cpp)

set_target_properties(awesome_api_lokal_indonesia_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${awesome_api_lokal_indonesia_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(awesome_api_lokal_indonesia_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${awesome_api_lokal_indonesia_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(awesome_api_lokal_indonesia_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET awesome_api_lokal_indonesia_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${awesome_api_lokal_indonesia_shared_dir}/${prefix_shared_library}awesome_api_lokal_indonesia${package_shared_extension}"
)

add_custom_command(
  TARGET awesome_api_lokal_indonesia_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${awesome_api_lokal_indonesia_exe_dir}/awesome_api_lokal_indonesia${package_exe_extension}" 
)

 