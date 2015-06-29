CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
	set(CMAKE_INSTALL_PREFIX ${CMAKE_BINARY_DIR} CACHE PATH "default install prefix" FORCE)
endif(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)

include(ExternalProject)

# Adding LCM as external project
ExternalProject_Add(lcm
	PREFIX lcm
#	URL http://lcm.googlecode.com/files/lcm-1.0.0.tar.gz
	URL https://github.com/lcm-proj/lcm/releases/download/v1.2.1/lcm-1.2.1.zip
	CONFIGURE_COMMAND ./../lcm/configure --prefix=${CMAKE_INSTALL_PREFIX}
	BUILD_COMMAND ${MAKE}
)

# Adding LBFGS as external project
#ExternalProject_Add(lbfgs
#	PREFIX lbfgs
#	# DOWNLOAD_COMMAND git clone https://github.com/chokkan/liblbfgs.git
#	URL https://github.com/downloads/chokkan/liblbfgs/liblbfgs-1.10.tar.gz
#	# URL http://www.chokkan.org/software/liblbfgs/liblbfgs-1.10.tar.gz
#	CONFIGURE_COMMAND ./../lbfgs/configure --prefix=${CMAKE_INSTALL_PREFIX}
#	BUILD_COMMAND ${MAKE}
#)


# Adding rocbot as an external
ExternalProject_Add(rocbot
	DEPENDS
	PREFIX rocbot
	DOWNLOAD_COMMAND svn co http://www.cs.rochester.edu/repos/rail/software/rocbot rocbot
	CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DCMAKE_MODULE_PATH=${CMAKE_INSTALL_PREFIX}/share/cmake-${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}/Modules -DBUILD_DOCS:BOOL=${BUILD_DOCS}
	BUILD_COMMAND ${MAKE}
)

# Adding h2sl as external (requires lbfgs and lcm)
ExternalProject_Add(h2sl
	# DEPENDS lcm lbfgs
	DEPENDS lcm
	PREFIX h2sl
	DOWNLOAD_COMMAND git clone https://code.google.com/p/h2sl/ h2sl
	CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DCMAKE_MODULE_PATH=${CMAKE_INSTALL_PREFIX}/share/cmake-${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}/Modules
)

install(TARGETS)
