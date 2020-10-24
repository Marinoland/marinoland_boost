cmake_minimum_required(VERSION 2.8)
project (boost)

if(NOT EXISTS ./ext)
	set(BOOSTDIR ${CMAKE_CURRENT_SOURCE_DIR}/ext)
	
	IF (WIN32)
	    set(SLASH \\)
	    set(SH_EXT bat)
	    set(BOOST_LIBVER -vc142-mt-x64-1_75)
	    set(LIBS )
	    set(LIBPREFIX lib)
	    set(BOOST_LIBVER -vc142-mt-x64-1_75)
	ELSE()
	    set(SLASH /)
	    set(SH_EXT sh)
	    set(BOOST_LIBVER )
	    set(LIBS pthread)
	    set(LIBPREFIX )
	    set(BOOST_LIBVER )
	ENDIF()

	file(WRITE depend.includedirs ${BOOSTDIR} \n ${BOOSTDIR}/libs/beast/include)
	file(WRITE depend.linkdirs ${BOOSTDIR}/stage/lib)
	file(WRITE depend.libs ${LIBS} \n
		${LIBPREFIX}boost_system${BOOST_LIBVER} \n ${LIBPREFIX}boost_filesystem${BOOST_LIBVER} \n
		${LIBPREFIX}boost_date_time${BOOST_LIBVER})

	execute_process(COMMAND git clone https://github.com/boostorg/boost.git ./ext)
	execute_process(COMMAND git submodule update --init --recursive WORKING_DIRECTORY ./ext)
	execute_process(COMMAND .${SLASH}bootstrap.${SH_EXT} WORKING_DIRECTORY ./ext )
	execute_process(COMMAND .${SLASH}b2 WORKING_DIRECTORY ./ext )
endif()
