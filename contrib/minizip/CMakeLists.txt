set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

add_library(minizip SHARED
	ioapi.c
	mztools.c
	unzip.c
	zip.c)

set(minizip_headers
	ioapi.h
	crypt.h
	mztools.h
	unzip.h
	zip.h)

if (WIN32)
	target_sources(minizip
		PRIVATE
		iowin32.c)
	list(APPEND minizip_headers iowin32.h)
endif()

target_link_libraries(minizip zlib)
target_compile_definitions(minizip PRIVATE -DIOAPI_NO_64=1)

include(GNUInstallDirs)
install(TARGETS minizip
	ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
	LIBRARY DESTINATION ${INSTALL_LIB_DIR}
	RUNTIME DESTINATION ${INSTALL_BIN_DIR}
	)
install(FILES ${minizip_headers}
	DESTINATION ${INSTALL_INC_DIR}/minizip)

