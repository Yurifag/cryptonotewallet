cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    ${GIT_SUBMODULE_PATH}/external
    ${GIT_SUBMODULE_PATH}/include
    ${GIT_SUBMODULE_PATH}/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("${GIT_SUBMODULE_PATH}/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/connecthostport.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/igd_desc_parse.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/minisoap.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/miniupnpc.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/miniwget.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/minixml.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/portlistingparse.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/receivedata.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/upnpcommands.c
  ${GIT_SUBMODULE_PATH}/external/miniupnpc/upnpreplyparse.c
  ${GIT_SUBMODULE_PATH}/src/Common/Base58.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/CommandLine.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/Util.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/StringTools.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/JsonValue.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/ConsoleTools.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/MemoryInputStream.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/PathTools.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/StdInputStream.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/StdOutputStream.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/StreamTools.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/StringOutputStream.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/StringView.cpp
  ${GIT_SUBMODULE_PATH}/src/Common/VectorOutputStream.cpp
  ${GIT_SUBMODULE_PATH}/src/crypto/blake256.c
  ${GIT_SUBMODULE_PATH}/src/crypto/chacha8.c
  ${GIT_SUBMODULE_PATH}/src/crypto/crypto-ops-data.c
  ${GIT_SUBMODULE_PATH}/src/crypto/crypto-ops.c
  ${GIT_SUBMODULE_PATH}/src/crypto/crypto.cpp
  ${GIT_SUBMODULE_PATH}/src/crypto/groestl.c
  ${GIT_SUBMODULE_PATH}/src/crypto/hash-extra-blake.c
  ${GIT_SUBMODULE_PATH}/src/crypto/hash-extra-groestl.c
  ${GIT_SUBMODULE_PATH}/src/crypto/hash-extra-jh.c
  ${GIT_SUBMODULE_PATH}/src/crypto/hash-extra-skein.c
  ${GIT_SUBMODULE_PATH}/src/crypto/hash.c
  ${GIT_SUBMODULE_PATH}/src/crypto/jh.c
  ${GIT_SUBMODULE_PATH}/src/crypto/keccak.c
  ${GIT_SUBMODULE_PATH}/src/crypto/oaes_lib.c
  ${GIT_SUBMODULE_PATH}/src/crypto/random.c
  ${GIT_SUBMODULE_PATH}/src/crypto/skein.c
  ${GIT_SUBMODULE_PATH}/src/crypto/slow-hash.c
  ${GIT_SUBMODULE_PATH}/src/crypto/slow-hash.cpp
  ${GIT_SUBMODULE_PATH}/src/crypto/tree-hash.c
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/BlockchainIndices.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/BlockchainMessages.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/BlockIndex.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/CoreConfig.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/CryptoNoteBasic.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/CryptoNoteTools.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Currency.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/MinerConfig.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Transaction.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Account.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Blockchain.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Checkpoints.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Core.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Difficulty.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/IBlock.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/Miner.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/TransactionExtra.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/TransactionPool.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteCore/TransactionUtils.cpp
  ${GIT_SUBMODULE_PATH}/src/InProcessNode/InProcessNode.cpp
  ${GIT_SUBMODULE_PATH}/src/InProcessNode/InProcessNodeErrors.cpp
  ${GIT_SUBMODULE_PATH}/src/NodeRpcProxy/NodeErrors.cpp
  ${GIT_SUBMODULE_PATH}/src/NodeRpcProxy/NodeRpcProxy.cpp
  ${GIT_SUBMODULE_PATH}/src/P2p/NetNodeConfig.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/BinaryInputStreamSerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/BinaryOutputStreamSerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/JsonInputValueSerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/JsonOutputStreamSerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/KVBinaryInputStreamSerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/Serialization/SerializationOverloads.cpp
  ${GIT_SUBMODULE_PATH}/src/Transfers/BlockchainSynchronizer.cpp
  ${GIT_SUBMODULE_PATH}/src/Transfers/SynchronizationState.cpp
  ${GIT_SUBMODULE_PATH}/src/Transfers/TransfersConsumer.cpp
  ${GIT_SUBMODULE_PATH}/src/Transfers/TransfersContainer.cpp
  ${GIT_SUBMODULE_PATH}/src/Transfers/TransfersSubscription.cpp
  ${GIT_SUBMODULE_PATH}/src/Transfers/TransfersSynchronizer.cpp
  ${GIT_SUBMODULE_PATH}/src/Wallet/LegacyKeysImporter.cpp
  ${GIT_SUBMODULE_PATH}/src/Wallet/WalletAsyncContextCounter.cpp
  ${GIT_SUBMODULE_PATH}/src/Wallet/WalletErrors.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/KeysStorage.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletLegacy.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletHelper.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletLegacySerializer.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletLegacySerialization.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletTransactionSender.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  ${GIT_SUBMODULE_PATH}/src/WalletLegacy/WalletUserTransactionsCache.cpp
  ${GIT_SUBMODULE_PATH}/src/System/ContextGroup.cpp
  ${GIT_SUBMODULE_PATH}/src/System/Event.cpp
  ${GIT_SUBMODULE_PATH}/src/System/EventLock.cpp
  ${GIT_SUBMODULE_PATH}/src/System/InterruptedException.cpp
  ${GIT_SUBMODULE_PATH}/src/System/Ipv4Address.cpp
  ${GIT_SUBMODULE_PATH}/src/System/TcpStream.cpp
  ${GIT_SUBMODULE_PATH}/src/HTTP/HttpRequest.cpp
  ${GIT_SUBMODULE_PATH}/src/HTTP/HttpParser.cpp
  ${GIT_SUBMODULE_PATH}/src/HTTP/HttpParserErrorCodes.cpp
  ${GIT_SUBMODULE_PATH}/src/HTTP/HttpResponse.cpp
  ${GIT_SUBMODULE_PATH}/src/Rpc/HttpClient.cpp
  ${GIT_SUBMODULE_PATH}/src/Rpc/JsonRpc.cpp
  ${GIT_SUBMODULE_PATH}/src/P2p/NetNode.cpp
  ${GIT_SUBMODULE_PATH}/src/P2p/LevinProtocol.cpp
  ${GIT_SUBMODULE_PATH}/src/P2p/NetNodeConfig.cpp
  ${GIT_SUBMODULE_PATH}/src/P2p/PeerListManager.cpp
  ${GIT_SUBMODULE_PATH}/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/ILogger.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/LoggerGroup.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/CommonLogger.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/LoggerManager.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/FileLogger.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/StreamLogger.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/ConsoleLogger.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/LoggerMessage.cpp
  ${GIT_SUBMODULE_PATH}/src/Logging/LoggerRef.cpp
  ${GIT_SUBMODULE_PATH}/src/BlockchainExplorer/BlockchainExplorer.cpp
  ${GIT_SUBMODULE_PATH}/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  ${GIT_SUBMODULE_PATH}/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(${GIT_SUBMODULE_PATH}/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${GIT_SUBMODULE_PATH}/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})

    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(${GIT_SUBMODULE_PATH}/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES ${GIT_SUBMODULE_PATH}/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
