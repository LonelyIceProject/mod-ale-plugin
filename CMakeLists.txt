include(cmake/FetchUpstream.cmake)
FetchContent_Declare(ale_lua52 URL https://www.lua.org/ftp/lua-5.2.4.tar.gz
  URL_HASH SHA256=b9e2e4aad6789b3b63a056d442f7b39f0ecfca3ae0f1fc0ae4e9614401b69f4b
  DOWNLOAD_EXTRACT_TIMESTAMP TRUE)
FetchContent_MakeAvailable(ale_lua52)
file(GLOB lua_sources "${ale_lua52_SOURCE_DIR}/src/*.c")
list(REMOVE_ITEM lua_sources "${ale_lua52_SOURCE_DIR}/src/lua.c" "${ale_lua52_SOURCE_DIR}/src/luac.c")
add_library(lonelyice-ale-lua STATIC ${lua_sources})
set_target_properties(lonelyice-ale-lua PROPERTIES POSITION_INDEPENDENT_CODE ON)
target_include_directories(lonelyice-ale-lua PUBLIC "${ale_lua52_SOURCE_DIR}/src")
if(UNIX)
  target_compile_definitions(lonelyice-ale-lua PRIVATE LUA_USE_DLOPEN LUA_USE_POSIX)
  target_link_libraries(lonelyice-ale-lua PRIVATE ${CMAKE_DL_LIBS} m)
endif()
file(GLOB_RECURSE sources CONFIGURE_DEPENDS "${module}/src/*.cpp")
list(FILTER sources EXCLUDE REGEX "/lualib/")
AddPlugin(mod-ale SOURCES ${sources} plugin/plugin.cpp LINK lonelyice-ale-lua)
target_include_directories(mod-ale PRIVATE "${module}/src/LuaEngine")
add_custom_target(mod-ale-upstream-files
  COMMAND ${CMAKE_COMMAND} -E copy_directory "${module}/conf" "${AC_PLUGINS_OUTPUT_DIR}/mod-ale/conf"
  COMMAND ${CMAKE_COMMAND} -E copy_directory "${module}/src/LuaEngine/extensions" "${AC_PLUGINS_OUTPUT_DIR}/mod-ale/lua"
  DEPENDS mod-ale-files VERBATIM)
add_dependencies(mod-ale mod-ale-upstream-files)
