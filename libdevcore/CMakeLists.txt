set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DSTATICLIB")

aux_source_directory(. SRC_LIST)
if(ENCRYPTTYPE)
	aux_source_directory(./sm3 SRC_LIST)
	include_directories(./sm3)
endif()
set(EXECUTABLE soldevcore)

file(GLOB HEADERS "*.h")

include_directories(..)
add_library(${EXECUTABLE} ${SRC_LIST} ${HEADERS})
target_include_directories(${EXECUTABLE} PUBLIC ${BOOST_INCLUDE_DIR})
target_link_libraries(${EXECUTABLE} Boost::Filesystem Boost::Random Boost::Thread ${LEVELDB_LIBRARIES})

eth_use(${EXECUTABLE} REQUIRED Dev::base)

install( TARGETS ${EXECUTABLE} RUNTIME DESTINATION bin ARCHIVE DESTINATION lib LIBRARY DESTINATION lib )
