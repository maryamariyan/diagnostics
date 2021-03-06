if(CLR_CMAKE_PLATFORM_UNIX)
    include_directories(${ROOT_DIR}/src/pal/inc)
    include_directories(${ROOT_DIR}/src/pal/inc/rt)

    add_subdirectory(lldbplugin)
endif(CLR_CMAKE_PLATFORM_UNIX)

# lldbplugin doesn't build with these options
if(WIN32)
  add_compile_options(/FIWarningControl.h) # force include of WarningControl.h
  add_compile_options(/Zl) # omit default library name in .OBJ
endif(WIN32)

add_definitions(-D_SECURE_SCL=0)

add_subdirectory(NETCore)
add_subdirectory(Strike)

if(CLR_CMAKE_PLATFORM_UNIX)
  add_compile_options(-fPIC)

  # Include the dummy c++ include files
  include_directories(${ROOT_DIR}/src/pal/inc/rt/cpp)

  # This prevents inclusion of standard C compiler headers
  add_compile_options(-nostdinc)
endif(CLR_CMAKE_PLATFORM_UNIX)

add_subdirectory(debugshim)
add_subdirectory(dbgutil)
