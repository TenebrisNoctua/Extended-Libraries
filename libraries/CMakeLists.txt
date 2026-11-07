set(TASK_ROOT ${CMAKE_CURRENT_LIST_DIR}/task)
set(FS_ROOT ${CMAKE_CURRENT_LIST_DIR}/fs)
set(IO_ROOT ${CMAKE_CURRENT_LIST_DIR}/io)
set(NET_ROOT ${CMAKE_CURRENT_LIST_DIR}/net)
set(SIGNAL_ROOT ${CMAKE_CURRENT_LIST_DIR}/signal)
set(BASEOBJECT_ROOT ${CMAKE_CURRENT_LIST_DIR}/baseobject)
set(COLOR_ROOT ${CMAKE_CURRENT_LIST_DIR}/color)
set(OS_ROOT ${CMAKE_CURRENT_LIST_DIR}/os)
set(PROCESS_ROOT ${CMAKE_CURRENT_LIST_DIR}/process)
set(RECT_ROOT ${CMAKE_CURRENT_LIST_DIR}/rect)
set(UDIM_ROOT ${CMAKE_CURRENT_LIST_DIR}/udim)
set(ZIP_ROOT ${CMAKE_CURRENT_LIST_DIR}/zip)
set(UDC_ROOT ${CMAKE_CURRENT_LIST_DIR}/udc)
set(LIBUV_ROOT ${CMAKE_CURRENT_LIST_DIR}/../extern/libuv)

set(LUWOW_LIBRARIES task fs io signal net baseobject color os process rect udim zip udc)

# Each library's DLL from its src folder
foreach(name IN LISTS LUWOW_LIBRARIES)
    add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/${name}/src ${name})
endforeach()

# Each library's types, scripts/<name>.luau, into this repository's types folder, the one .config.luau's Luwow
# alias names, and into a types folder besides the libraries folder when installed
luwow_copy_library_types(${LUWOW_LIBRARIES}
    ROOT ${CMAKE_CURRENT_LIST_DIR}
    DESTINATION ${CMAKE_CURRENT_LIST_DIR}/../types
    INSTALL_COMPONENT libraries
)

# Only the static uv_a is linked into task, skip building uv.dll and libuv's tests
set(LIBUV_BUILD_SHARED OFF CACHE BOOL "" FORCE)
set(LIBUV_BUILD_TESTS OFF CACHE BOOL "" FORCE)
add_subdirectory(${LIBUV_ROOT} libuv)
