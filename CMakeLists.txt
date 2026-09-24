enable_language(C)

set(LUA_CORE_SOURCES
    lapi.c
    lcode.c
    lctype.c
    ldebug.c
    ldo.c
    ldump.c
    lfunc.c
    lgc.c
    llex.c
    lmem.c
    lobject.c
    lopcodes.c
    lparser.c
    lstate.c
    lstring.c
    ltable.c
    ltm.c
    lundump.c
    lvm.c
    lzio.c
    lauxlib.c
    lbaselib.c
    lcorolib.c
    ldblib.c
    liolib.c
    lmathlib.c
    loadlib.c
    loslib.c
    lstrlib.c
    ltablib.c
    lutf8lib.c
    linit.c
)

add_library(lua SHARED ${LUA_CORE_SOURCES})

add_library(Chicane::Lua ALIAS lua)

set_target_properties(
    lua
    PROPERTIES
        OUTPUT_NAME lua
        POSITION_INDEPENDENT_CODE TRUE
        C_STANDARD 99
        C_STANDARD_REQUIRED ON
)

target_include_directories(
    lua
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
)

target_compile_definitions(
    lua
    PRIVATE
        LUA_BUILD_AS_DLL
        LUA_CORE
        _CRT_SECURE_NO_WARNINGS
    INTERFACE
        LUA_BUILD_AS_DLL
)

if (APPLE)
    target_compile_definitions(lua PRIVATE LUA_USE_MACOSX)
elseif (UNIX)
    target_compile_definitions(lua PRIVATE LUA_USE_LINUX)
endif()

if (MSVC)
    target_compile_options(lua PRIVATE /W0)
else()
    target_compile_options(lua PRIVATE -w)
endif()

if (UNIX)
    target_link_libraries(lua PUBLIC m)
    if (NOT APPLE)
        target_link_libraries(lua PUBLIC ${CMAKE_DL_LIBS})
    endif()
endif()
