cmake_minimum_required(VERSION 3.23)

project(VulkanMemoryAllocator-Hpp
    VERSION 0
    DESCRIPTION "C++ bindings for the VulkanMemoryAllocator library."
    LANGUAGES C CXX
)

find_package(Vulkan REQUIRED)

if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/VulkanMemoryAllocator/CMakeLists.txt)
    add_subdirectory(VulkanMemoryAllocator)
else()
    find_package(VulkanMemoryAllocator REQUIRED)
endif()

add_library(VulkanMemoryAllocator-Hpp INTERFACE)
add_library(VulkanMemoryAllocator-Hpp::VulkanMemoryAllocator-Hpp ALIAS VulkanMemoryAllocator-Hpp)

target_sources(VulkanMemoryAllocator-Hpp
	INTERFACE
		FILE_SET HEADERS
		BASE_DIRS
			include
		FILES
			include/vk_mem_alloc_enums.hpp
			include/vk_mem_alloc_funcs.hpp
			include/vk_mem_alloc_handles.hpp
			include/vk_mem_alloc_structs.hpp
			include/vk_mem_alloc.hpp
)

target_link_libraries(VulkanMemoryAllocator-Hpp
    INTERFACE
        Vulkan::Vulkan
        GPUOpen::VulkanMemoryAllocator
)

if(PROJECT_IS_TOP_LEVEL)
	include(GNUInstallDirs)

	install(
		TARGETS
			VulkanMemoryAllocator-Hpp
		EXPORT
			VulkanMemoryAllocator-HppTargets
		FILE_SET HEADERS
		INCLUDES DESTINATION
			${CMAKE_INSTALL_INCLUDEDIR}
	)

	install(
		EXPORT
			VulkanMemoryAllocator-HppTargets
		FILE
			VulkanMemoryAllocator-HppTargets.cmake
		NAMESPACE
			VulkanMemoryAllocator-Hpp::
		DESTINATION
			${CMAKE_INSTALL_LIBDIR}/cmake/VulkanMemoryAllocator-Hpp
	)

	install(
		FILES
			${CMAKE_CURRENT_SOURCE_DIR}/cmake/VulkanMemoryAllocator-Hpp
		DESTINATION
			${CMAKE_INSTALL_LIBDIR}/cmake/VulkanMemoryAllocator-Hpp
	)
endif()
