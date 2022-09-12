cmake_minimum_required(VERSION 2.8)
project (boost)

IF (WIN32)
	set(SH_EXT .bat)
	set(EXE_EXT .exe)
	set(SLASH \\)
	string(REPLACE v vc toolset ${CMAKE_VS_PLATFORM_TOOLSET})
	set(BOOTSTRAP_ARGS ${toolset})

	string(REPLACE v "" tmp ${CMAKE_VS_PLATFORM_TOOLSET})
	string(SUBSTRING ${tmp} 0 2 part1)
	string(SUBSTRING ${tmp} 2 1 part2)
	set(B2_ARGS toolset=msvc-${part1}.${part2})
ELSE()
	set(SH_EXT .sh)
	set(EXE_EXT )
	set(SLASH /)
	set(BOOTSTRAP_ARGS )
	set(B2_ARGS )
ENDIF()

file(GLOB outputfiles "${CMAKE_BINARY_DIR}/libboost_*")
list(LENGTH outputfiles numoutputfiles)
if(numoutputfiles EQUAL 0)

	execute_process(COMMAND git submodule init)
	execute_process(COMMAND git submodule update --init --recursive)

	execute_process(COMMAND .${SLASH}build${SH_EXT} ${BOOTSTRAP_ARGS} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext/tools/build/src/engine )
	file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/ext/tools/build/src/engine/b2${EXE_EXT}
		DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/ext/
	)
	# went direct to "build" due to bootstrap incorrectly determining the toolset
	#execute_process(COMMAND .${SLASH}bootstrap${SH_EXT} ${BOOTSTRAP_ARGS} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
	execute_process(COMMAND .${SLASH}b2${EXE_EXT} ${B2_ARGS} address-model=64 WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )

	execute_process(COMMAND rm -rf ${CMAKE_BINARY_DIR})
	file(COPY 
		${CMAKE_CURRENT_SOURCE_DIR}/ext/stage/lib/
		DESTINATION ${CMAKE_BINARY_DIR}
	)
	
endif()
