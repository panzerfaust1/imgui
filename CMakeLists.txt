cmake_minimum_required(VERSION 3.20)
project(imgui)

# options.
option(IM_SHARED "Enable shared library" OFF)
option(IM_BACKEND_RENDERER "Renderer backend to use" "")
option(IM_BACKEND_INPUT "Input backend to use" "")

# test renderer file to exist.
if (IM_BACKEND_RENDERER)
	if (NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_${IM_BACKEND_RENDERER}.h)
		message(FATAL_ERROR "Backend renderer '${IM_BACKEND_RENDERER}' not found.")
	endif()
else()
    message(FATAL_ERROR "IM_BACKEND_RENDERER is not set.")
endif()

# test input file to exist.
if (IM_BACKEND_INPUT)
	if (NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_${IM_BACKEND_INPUT}.h)
		message(FATAL_ERROR "Backend input '${IM_BACKEND_INPUT}' not found.")
	endif()
else()
	message(FATAL_ERROR "IM_BACKEND_INPUT is not set.")
endif()

# set up include directories.
include_directories(.)
include_directories(backends)

# set up base source files.
set(_IM_SOURCE
	imgui.cpp
	imgui_draw.cpp
	imgui_tables.cpp
	imgui_widgets.cpp
)

# set up renderer and input backend.
set(_IM_SOURCE_BACKEND backends/imgui_impl_${IM_BACKEND_RENDERER}.cpp backends/imgui_impl_${IM_BACKEND_INPUT}.cpp)

# deduct whether this is a shared or static library.
if (IM_SHARED)
	set(_IM_BUILD_TYPE SHARED)
else()
	set(_IM_BUILD_TYPE STATIC)
endif()

# add source files.
add_library(imgui ${_IM_BUILD_TYPE} ${_IM_SOURCE} ${_IM_SOURCE_BACKEND})
