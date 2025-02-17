cmake_minimum_required(VERSION 2.6)
PROJECT(Base)

AUX_SOURCE_DIRECTORY(./ SRC_LIST1)
AUX_SOURCE_DIRECTORY(./pb/protocol SRC_LIST2)
AUX_SOURCE_DIRECTORY(./jsoncpp SRC_LIST3)

# CMAKE的输出路径为TeamTalk\server\src\bin\${name}，${name}代表项目名称，如login_server
## ../../定位到src下，并且只需要连接静态库时，增加该前缀
SET(SRC_DIR  ../../)
SET(PB_INCLUDE_DIR ../protobuf/include)
SET(PB_LIB_DIR ${SRC_DIR}/protobuf/lib)

SET(SLOG_INCLUDE_DIR ../slog)
SET(SLOG_LIB_DIR ${SRC_DIR}/lib)


SET(LIBRARY_OUTPUT_PATH ./)
#SET(EXECUTABLE_OUTPUT_PATH  ./bin)

ADD_DEFINITIONS( -g -W -Wall -D_REENTRANT -D_FILE_OFFSET_BITS=64 -DAC_HAS_INFO
-DAC_HAS_WARNING -DAC_HAS_ERROR -DAC_HAS_CRITICAL -DTIXML_USE_STL
-DAC_HAS_DEBUG -DLINUX_DAEMON)


#设置自己mac os的openssl的路径
if(APPLE)
    SET(OPENSSL_INCLUDE_DIR /usr/local/Cellar/openssl@1.1/1.1.1j/include)
    SET(OPENSSL_LIB_DIR /usr/local/Cellar/openssl@1.1/1.1.1j/lib)
endif()

INCLUDE_DIRECTORIES(./ ../ ./jsoncpp ${PB_INCLUDE_DIR} ./pb/protocol ${OPENSSL_INCLUDE_DIR} ${SLOG_INCLUDE_DIR})
LINK_DIRECTORIES(./ ${PB_LIB_DIR} ${SLOG_LIB_DIR} ${OPENSSL_LIB_DIR})

ADD_LIBRARY(base STATIC ${SRC_LIST1} ${SRC_LIST2} ${SRC_LIST3})

#ADD_XXXX必须在TARGET_LINK_LIBRARIES前面，否则会报错
#ADD_LIBRARY(${PROJECTNAME} SHARED ${SRC_LIST})
#ADD_EXECUTABLE(${PROJECTNAME} ${SRC_LIST})

TARGET_LINK_LIBRARIES(base pthread slog crypto ssl)
