cmake_minimum_required(VERSION 2.8)
project (boost)

execute_process(COMMAND git submodule init)
execute_process(COMMAND git submodule update --init --recursive)
	
IF (WIN32)
	set(SH_EXT bat)
ELSE()
	set(SH_EXT sh)
ENDIF()

execute_process(COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/ext/bootstrap.${SH_EXT} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
execute_process(COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/ext/b2 WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )

execute_process(COMMAND rm -rf ${CMAKE_CURRENT_BINARY_DIR}/boost)
execute_process(COMMAND mv
	${CMAKE_CURRENT_SOURCE_DIR}/ext/stage/lib/
	${CMAKE_CURRENT_BINARY_DIR}/boost
)
