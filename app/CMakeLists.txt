cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DHAVE_INTTYPES_H")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DHAVE_INTTYPES_H")
add_library(native-lib SHARED src/main/cpp/jni.c)

find_library(log-lib log)
target_link_libraries(native-lib ${log-lib} )

add_library(crypto STATIC IMPORTED)
set_target_properties(crypto PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/src/main/cpp/openssl/libcrypto.a)

add_library(ssl STATIC IMPORTED)
set_target_properties(ssl PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/src/main/cpp/openssl/libssl.a)

add_library(vpx STATIC IMPORTED)
set_target_properties(vpx PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/src/main/cpp/vpx/libvpx.a)

add_library(opus STATIC IMPORTED)
set_target_properties(opus PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/src/main/cpp/opus/libopus.a)

add_subdirectory(cmake/re)
add_subdirectory(cmake/rem)
add_subdirectory(cmake/ilbc)
add_subdirectory(cmake/baresip)

target_link_libraries(native-lib "re")
target_link_libraries(native-lib "rem")
target_link_libraries(native-lib "baresip")
