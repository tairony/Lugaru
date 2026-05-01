cmake_minimum_required(VERSION 3.25.0)
project(Lugaru)

set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT Lugaru)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set(CXX_STANDARD_REQUIRED ON)

if (MSVC)
	# Ignore warnings about missing pdb
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /ignore:4099")
	set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /ignore:4099")
	set(CMAKE_STATIC_LINKER_FLAGS "${CMAKE_STATIC_LINKER_FLAGS} /ignore:4099")
endif()

find_package(OpenGL REQUIRED)
find_package(OpenAL REQUIRED)
find_package(JPEG REQUIRED)
find_package(PNG REQUIRED)
find_package(SDL2 REQUIRED)
find_package(PkgConfig REQUIRED)

# Mova todo o bloco que busca e vincula a biblioteca vorbis para depois da última linha add_subdirectory(Lugaru)

# Exemplo:

# if (VORBIS_FOUND)
# endif()
find_package(jsoncpp REQUIRED)
# find_package(glext REQUIRED)  # headers in mesa-libGL-devel
# find_package(glu REQUIRED)  # headers in mesa-libGL-devel

# 
add_subdirectory(Foundation)
add_subdirectory(Audio)
add_subdirectory(Graphics)
add_subdirectory(Game)
add_subdirectory(App)
add_subdirectory(Lugaru)

# === VORBIS LINKING FIX (Fedora 43) ===
# Executado APÓS os subdiretórios para garantir que o target existe
if (VORBIS_FOUND AND TARGET Lugaru)
    target_link_libraries(Lugaru PRIVATE ${VORBIS_LIBRARIES})
endif()
# === END FIX ===

# Link explícito para GLU e Vorbis (Fedora/Linux)
target_link_libraries(Lugaru PRIVATE GLU vorbisfile ${VORBIS_LIBRARIES})
