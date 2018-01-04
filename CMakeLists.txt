set (CMAKE_CXX_STANDARD 14)
cmake_minimum_required(VERSION 3.7)

# From the pubnub src, I uncommented extern c from pubnub_common.hpp, pubnub_futres_cpp11.cpp
# pubnub_futres_posix.cpp, pubnub_futres_sync.cpp, and pubnub.hpp in cpp
add_definitions(-DPUBNUB_LOG_LEVEL=PUBNUB_LOG_LEVEL_WARNING)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DVERSION=\"\\\"${GIT_VERSION}\\\"\"")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -ggdb3")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELEASE} -ggdb3")

FILE(GLOB CPP_HEADERS ${PROJECT_SOURCE_DIR}/cpp/*.h ${PROJECT_SOURCE_DIR}/cpp/*.hpp)
FILE(GLOB CORE_HEADERS ${PROJECT_SOURCE_DIR}/core/*.h)
FILE(GLOB POSIX_HEADERS ${PROJECT_SOURCE_DIR}/posix/*.h)
FILE(GLOB BASE64_HEADERS ${PROJECT_SOURCE_DIR}/lib/base64/*.h)
FILE(GLOB MD5_HEADERS ${PROJECT_SOURCE_DIR}/lib/md5/*.h)
SET(OPENSSL_HEADERS openssl/pubnub_config.h)
SET(ALL_HEADERS ${CPP_HEADERS} ${CORE_HEADERS} ${POSIX_HEADERS} ${BASE64_HEADERS} ${MD5_HEADERS} ${OPENSSL_HEADERS})

SET(PUBNUB_SOURCE core/pubnub_pubsubapi.c
        core/pubnub_coreapi.c
        core/pubnub_coreapi_ex.c
        core/pubnub_ccore_pubsub.c
        core/pubnub_ccore.c
        core/pubnub_netcore.c
        lib/sockets/pbpal_sockets.c
        lib/sockets/pbpal_resolv_and_connect_sockets.c
        core/pubnub_alloc_std.c
        core/pubnub_assert_std.c
        core/pubnub_generate_uuid.c
        core/pubnub_blocking_io.c
        core/pubnub_timers.c
        core/pubnub_json_parse.c
        lib/md5/md5.c
        lib/base64/pbbase64.c
        core/pubnub_helper.c
        posix/pubnub_version_posix.c
        posix/pubnub_generate_uuid_posix.c
        posix/pbpal_posix_blocking_io.c
        core/pubnub_free_with_timeout_std.c
        cpp/pubnub_subloop.cpp
        core/pubnub_proxy.c
        core/pubnub_proxy_core.c
        core/pbhttp_digest.c
        core/pbntlm_core.c
        core/pbntlm_packer_std.c
        posix/monotonic_clock_get_time_posix.c
        core/pubnub_ntf_sync.c
        cpp/pubnub_futres_sync.cpp
        cpp/pubnub_futres_cpp11.cpp
        )

include_directories(pubnub PUBLIC ${PROJECT_SOURCE_DIR}/cpp)
include_directories(pubnub PUBLIC ${PROJECT_SOURCE_DIR}/core)
include_directories(pubnub PUBLIC ${PROJECT_SOURCE_DIR}/posix)
include_directories(pubnub PUBLIC ${PROJECT_SOURCE_DIR}/lib/base64)
include_directories(pubnub PUBLIC ${PROJECT_SOURCE_DIR}/lib/md5)

add_library(pubnub ${PUBNUB_SOURCE})

install(DIRECTORY DESTINATION /usr/local/lib/)
install(DIRECTORY DESTINATION /usr/local/include/pubnub)

install(TARGETS pubnub DESTINATION /usr/local/lib/)
install(FILES ${ALL_HEADERS} DESTINATION /usr/local/include/pubnub)

