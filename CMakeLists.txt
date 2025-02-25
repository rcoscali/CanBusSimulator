cmake_minimum_required(VERSION 3.14)
project(Multicast-Client-Server-Example)

if(UNIX)
    add_definitions(-DUNIX)
endif(UNIX)

if(WIN32)
    add_definitions(-DWIN32)
    if(WINCE)
      set(ADDITIONAL_LIBS ${ADDITIONAL_LIBS} ws2)
    else()
      set(ADDITIONAL_LIBS ${ADDITIONAL_LIBS} ws2_32)
    endif()
    # MSVC older than VS 14.0 does not support the C99-style variable declarations in the middle of a block like we have them.
    # A workaround is to tell the compiler to treat C sources as C++ ones.
    if(MSVC AND MSVC_VERSION LESS 1900)
      file(GLOB_RECURSE c_sources "*.c")
      set_source_files_properties(${c_sources} PROPERTIES LANGUAGE CXX)
    endif()
endif(WIN32)


# the server

SET(server_SRCS
   src/server.c
   src/msock.c
)

add_executable(server ${server_SRCS})

target_link_libraries(server
                      ${ADDITIONAL_LIBS}
)

install(TARGETS server DESTINATION bin)


# the client

SET(client_SRCS
   src/client.c
   src/msock.c
)

add_executable(client ${client_SRCS})

target_link_libraries(client
                      ${ADDITIONAL_LIBS}
)

install(TARGETS client DESTINATION bin)

