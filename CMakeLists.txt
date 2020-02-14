cmake_minimum_required(VERSION 3.10)
#set(CMAKE_VERBOSE_MAKEFILE ON)
project(webbench_1_5)

set(CMAKE_CXX_STANDARD 11)
set(CFLAGS "-Wall -ggdb -W -O")
set(CC "gcc")
set(VERSION "1.5")


# $ make
add_library(Socket socket.c)
add_executable(webbench webbench.c)
link_libraries(Socket)


# $ make install
install(TARGETS webbench DESTINATION bin)
install(FILES webbench.1 DESTINATION man/man1
        PERMISSIONS WORLD_READ OWNER_WRITE)
install(DIRECTORY DESTINATION share/doc/webbench)
install(FILES debian/copyright debian/changelog DESTINATION share/doc/webbench
        PERMISSIONS WORLD_READ OWNER_WRITE)


# $ make clean-all
set(FILE_PATHS
        ${CMAKE_INSTALL_PREFIX}/bin/webbench
        ${CMAKE_INSTALL_PREFIX}/man/man1/webbench.1
        ${CMAKE_INSTALL_PREFIX}/share/doc/webbench/copyright
        ${CMAKE_INSTALL_PREFIX}/share/doc/webbench/changelog
)
add_custom_target(clean-all)
foreach(file_path ${FILE_PATHS})
    add_custom_command(TARGET clean-all
                POST_BUILD
                COMMAND /bin/sh -c "\
               if test -f '${file_path}' ;\
               then \
                    rm -rf ${file_path} ;\
                    echo '${file_path} is been deleted' ;\
               fi;"
                VERBATIM
            )
endforeach()






