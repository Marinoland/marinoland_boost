cmake_minimum_required(VERSION 2.8)
project (boost)

execute_process(COMMAND git submodule init)
execute_process(COMMAND git submodule update --init --recursive)
	
IF (WIN32)
	set(SH_EXT .bat)
	set(EXE_EXT .exe)
	set(SLASH \\)
ELSE()
	set(SH_EXT .sh)
	set(EXE_EXT )
	set(SLASH /)
ENDIF()

execute_process(COMMAND .${SLASH}bootstrap${SH_EXT} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
execute_process(COMMAND .${SLASH}b2${EXE_EXT} address-model=64 WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )

execute_process(COMMAND rm -rf ${CMAKE_CURRENT_BINARY_DIR})
execute_process(COMMAND cp
	${CMAKE_CURRENT_SOURCE_DIR}/ext/stage/lib/
	${CMAKE_CURRENT_BINARY_DIR}
)
