set (PLUGIN_NAME mosquitto_force_retain)

add_library(${PLUGIN_NAME} MODULE
	${PLUGIN_NAME}.c
)

target_include_directories(${PLUGIN_NAME} PRIVATE
	"${OPENSSL_INCLUDE_DIR}"
	"${mosquitto_SOURCE_DIR}"
	"${mosquitto_SOURCE_DIR}/include"
)

set_target_properties(${PLUGIN_NAME} PROPERTIES
	PREFIX ""
	POSITION_INDEPENDENT_CODE 1
)

target_link_libraries(${PLUGIN_NAME} PRIVATE mosquitto)

install(TARGETS ${PLUGIN_NAME} RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}")