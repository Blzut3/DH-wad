cmake_minimum_required(VERSION 2.6)

project(DH-wad)

include(CheckCXXCompilerFlag)



# Simplify flag checking and adding.
function(TRY_CXX_COMPILER_FLAG flag name)
   CHECK_CXX_COMPILER_FLAG(${flag} ${name})

   if(${name})
      add_definitions(${flag})
   endif()
endfunction()

# I would really prefer a generalized condition instead of having to explicitly
# list IDs. (At least, a way to specify multiple IDs per invocation.)
function(TRY_CXX_COMPILER_FLAG_ID flag name id)
   if(CMAKE_CXX_COMPILER_ID STREQUAL ${id})
      TRY_CXX_COMPILER_FLAG(${flag} ${name})
   endif()
endfunction()


# Warnings.
TRY_CXX_COMPILER_FLAG_ID(+w2                FLAG_CXX_w2                "SunPro")
TRY_CXX_COMPILER_FLAG_ID(-Wall              FLAG_CXX_Wall              "Clang" )
TRY_CXX_COMPILER_FLAG_ID(-Wall              FLAG_CXX_Wall              "GNU"   )
TRY_CXX_COMPILER_FLAG_ID(-Wall              FLAG_CXX_Wall              "Intel" )
TRY_CXX_COMPILER_FLAG_ID(-Wcheck            FLAG_CXX_Wcheck            "Intel" )
TRY_CXX_COMPILER_FLAG_ID(-Wextra            FLAG_CXX_Wextra            "Clang" )
TRY_CXX_COMPILER_FLAG_ID(-Wextra            FLAG_CXX_Wextra            "GNU"   )
TRY_CXX_COMPILER_FLAG_ID(-Wp64              FLAG_CXX_Wp64              "Intel" )
TRY_CXX_COMPILER_FLAG_ID(-Wshadow           FLAG_CXX_Wshadow           "Clang" )
TRY_CXX_COMPILER_FLAG_ID(-Wshadow           FLAG_CXX_Wshadow           "GNU"   )
TRY_CXX_COMPILER_FLAG_ID(-Wshorten-64-to-32 FLAG_CXX_Wshorten_64_to_32 "Intel" )
TRY_CXX_COMPILER_FLAG_ID(-ansi              FLAG_CXX_ansi              "GNU"   )



if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
	add_definitions(-DTARGET_OS_WIN32)
else()
	add_definitions(-DTARGET_OS_POSIX)
endif()



add_subdirectory(src)

