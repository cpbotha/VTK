if (NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/vtk-m/CMakeLists.txt")
  message(FATAL_ERROR "VTKm requested, but the VTKm submodule is not initialized.\n"
    "Please run 'git submodule update --init --recursive' in the source directory.")
endif()

set(vtk_using_tbb OFF)
set(vtkm_components_to_find Serial)
if (VTK_SMP_IMPLEMENTATION_TYPE STREQUAL "TBB")
  set(vtk_using_tbb ON)
  list(APPEND vtkm_components_to_find TBB)
endif()

set(VTKm_ENABLE_TBB ${vtk_using_tbb} CACHE BOOL "Set to match VTK" FORCE)
set(VTKm_ENABLE_TESTING OFF CACHE BOOL "Disabled when building as part of VTK" FORCE)
set(VTKm_ENABLE_RENDERING OFF CACHE BOOL "Disabled when building as part of VTK" FORCE)
set(VTKm_USE_64BIT_IDS ${VTK_USE_64BIT_IDS} CACHE BOOL "Set to match VTK" FORCE)

if (WIN32)
  # Force the dlls to be in the bin folder with VTK's so that running tests
  # don't need to set path on Windows
  set(VTKm_EXECUTABLE_OUTPUT_PATH "bin" CACHE PATH "Set to match VTK" FORCE)
endif()
set(VTKm_INSTALL_INCLUDE_DIR "include/vtk-${VTK_MAJOR_VERSION}.${VTK_MINOR_VERSION}")
set(VTKm_INSTALL_CMAKE_MODULE_DIR "lib/cmake/vtk-${VTK_MAJOR_VERSION}.${VTK_MINOR_VERSION}/vtkm")

add_subdirectory(vtk-m)

set(vtkm_INCLUDE_DIRS ${CMAKE_CURRENT_BINARY_DIR}/vtk-m/include)
set(vtkm_LIBRARIES vtkm vtkm_cont)
vtk_module_export_info()
