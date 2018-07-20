find_package(Protobuf REQUIRED)

include_directories(${PROTOBUF_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${GLOW_THIRDPARTY_DIR}/onnx)

add_definitions(-DGOOGLE_PROTOBUF_NO_RTTI)

PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS ${GLOW_THIRDPARTY_DIR}/onnx/onnx/onnx.proto)

add_library(onnxifi-glow SHARED
              onnxifiGlowBackend.cpp
              ${PROTO_SRCS}
              ${PROTO_HDRS})
target_compile_definitions(onnxifi-glow
                           INTERFACE
                             -DGOOGLE_PROTOBUF_NO_RTTI)
target_link_libraries(onnxifi-glow
                      PRIVATE
			Importer
                        ${PROTOBUF_LIBRARY})
