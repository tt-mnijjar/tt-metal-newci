if(ENABLE_COVERAGE AND CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    message(STATUS "Enabling code coverage flags for all tt_metal targets")
    add_compile_options(--coverage)
    add_link_options(--coverage)
endif()

add_library(tt_metal)
add_library(Metalium::Metal ALIAS tt_metal)

target_sources(
    tt_metal
    PRIVATE
        tt_metal.cpp
        graph/graph_tracking.cpp
        hal.cpp
)

# TODO(afuller): this should be self-describing modules.
#   For now just cherry-pick all the files I discovered empirally by trying to run a test.
add_library(jitapi INTERFACE)
target_sources(
    jitapi
    INTERFACE
        FILE_SET jit_api
        TYPE HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
        FILES
            api/tt-metalium/circular_buffer_constants.h
            api/tt-metalium/dev_msgs.h
            api/tt-metalium/fabric_host_interface.h
            core_descriptors/blackhole_140_arch.yaml
            core_descriptors/wormhole_b0_80_arch.yaml
            impl/dispatch/kernels/cq_commands.hpp
            impl/dispatch/kernels/cq_common.hpp
            impl/dispatch/kernels/cq_helpers.hpp
            impl/dispatch/kernels/packet_queue.hpp
            impl/dispatch/kernels/packet_queue_ctrl.hpp
            include/compute_kernel_api/blank.h
            include/compute_kernel_api/cb_api.h
            include/compute_kernel_api/common.h
            include/compute_kernel_api/common_globals.h
            include/compute_kernel_api/eltwise_unary/eltwise_unary.h
            include/compute_kernel_api/eltwise_unary/exp.h
            include/compute_kernel_api/eltwise_unary/sfpu_split_includes.h
            include/compute_kernel_api/matmul.h
            include/compute_kernel_api/pack.h
            include/compute_kernel_api/reconfig_data_format.h
            include/compute_kernel_api/reg_api.h
            include/compute_kernel_api/tile_move_copy.h
            soc_descriptors/blackhole_140_arch.yaml
            soc_descriptors/wormhole_b0_80_arch.yaml
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_addrmod.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_defs.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_globals.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_gpr_map.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_include.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_instr_params.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_ops.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_sfpu.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_structs.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/ckernel_template.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/cmath_common.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/cpack_common.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/cunpack_common.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_abs.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_add_int32.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_binary.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_binary_bitwise.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_cast_fp32_to_fp16a.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_clamp.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_comp.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_cumsum.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_dropout.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_exp.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_gelu.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_hardtanh.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_is_fp16_zero.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_is_fp16_zero.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_load_config.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_load_config.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_log.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_max.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_max_int32.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_power.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_quant.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_recip.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_recip.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_relu.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_shift.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_sigmoid.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_sign.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_sqrt.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_square.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_sub_int32.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_tanh.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_tanh_derivative.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_topk.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_trigonometry.h
            third_party/tt_llk/tt_llk_blackhole/common/inc/sfpu/ckernel_sfpu_typecast.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_defs.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_math_common.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_math_eltwise_unary_datacopy.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_math_eltwise_unary_sfpu.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_math_matmul.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_pack.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_pack_common.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_pack_untilize.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_unpack_A.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_unpack_AB_matmul.h
            third_party/tt_llk/tt_llk_blackhole/llk_lib/llk_unpack_common.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_addrmod.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_defs.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_globals.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_gpr_map.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_include.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_instr_params.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_ops.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_sfpu.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_structs.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/ckernel_template.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/cmath_common.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/cpack_common.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/cunpack_common.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_abs.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_add_int32.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_binary.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_binary_bitwise.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_cast_fp32_to_fp16a.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_clamp.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_comp.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_cumsum.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_dropout.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_exp.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_gelu.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_hardtanh.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_is_fp16_zero.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_is_fp16_zero.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_load_config.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_load_config.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_log.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_max.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_max_int32.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_power.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_quant.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_recip.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_recip.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_relu.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_shift.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_sigmoid.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_sign.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_sqrt.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_square.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_sub_int32.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_tanh.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_tanh_derivative.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_topk.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_trigonometry.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_typecast.h
            third_party/tt_llk/tt_llk_wormhole_b0/common/inc/sfpu/ckernel_sfpu_reshuffle_rows.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_defs.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_math_common.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_math_eltwise_unary_datacopy.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_math_eltwise_unary_sfpu.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_math_matmul.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_pack.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_pack_common.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_pack_untilize.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_unpack_A.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_unpack_AB_matmul.h
            third_party/tt_llk/tt_llk_wormhole_b0/llk_lib/llk_unpack_common.h
            tools/profiler/kernel_profiler.hpp
            # Kernel sources
            impl/dispatch/kernels/cq_dispatch.cpp
            impl/dispatch/kernels/cq_dispatch_slave.cpp
            impl/dispatch/kernels/cq_prefetch.cpp
            impl/dispatch/kernels/packet_demux.cpp
            impl/dispatch/kernels/vc_packet_router.cpp
            impl/dispatch/kernels/vc_eth_tunneler.cpp
            impl/dispatch/kernels/packet_mux.cpp
            kernels/compute/blank.cpp
            kernels/compute/eltwise_binary.cpp
            kernels/compute/eltwise_sfpu.cpp
            kernels/dataflow/blank.cpp
            kernels/dataflow/reader_binary_diff_lengths.cpp
            kernels/dataflow/reader_unary.cpp
            kernels/dataflow/writer_unary.cpp
            kernels/dataflow/writer_unary_1.cpp
)

target_link_libraries(
    tt_metal
    PUBLIC
        umd::device
        magic_enum::magic_enum
        fmt::fmt-header-only
        span
        small_vector
        TracyClient
        nlohmann_json::nlohmann_json
        TT::Metalium::HostDevCommon
        Reflect::Reflect
        TT:STL
    PRIVATE
        Metalium::Metal::Impl
        metal_common_libs
        common
        jit_build
        llrt
        detail
        distributed
        fabric
        HAL::wormhole
        HAL::blackhole
        FlatBuffers::FlatBuffers
)

target_precompile_headers(
    tt_metal
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/third_party/tracy/public/tracy/Tracy.hpp
        <functional>
        <map>
        <memory>
        <unordered_map>
        <variant>
        <vector>
)

target_include_directories(
    tt_metal
    PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/api>"
    PRIVATE
        api/tt-metalium # FIXME: Re-home the tests and remove this
)
target_compile_options(tt_metal PUBLIC -Wno-int-to-pointer-cast)
add_dependencies(tt_metal hw_toolchain)

set_target_properties(
    tt_metal
    PROPERTIES
        INSTALL_RPATH
            "${PROJECT_BINARY_DIR}/lib;$ORIGIN"
        ADDITIONAL_CLEAN_FILES
            "${PROJECT_BINARY_DIR}/lib;${PROJECT_BINARY_DIR}/obj"
)

if(BUILD_PROGRAMMING_EXAMPLES)
    add_subdirectory(programming_examples)
endif()

# Allow internal files to access the public API "by default" and without the
# scoping that external consumers must use.  Scoping may still be used if desired.
include_directories(
    api
    api/tt-metalium
)

add_subdirectory(hw)
add_subdirectory(hostdevcommon)
add_subdirectory(common)
add_subdirectory(jit_build)
add_subdirectory(llrt)
add_subdirectory(tools)
add_subdirectory(impl)
add_subdirectory(detail)
add_subdirectory(distributed)
add_subdirectory(fabric)
if(TT_METAL_BUILD_TESTS)
    add_subdirectory(test)
endif()

install(
    TARGETS
        tt_metal
        jitapi
    LIBRARY
        COMPONENT metalium-runtime
    FILE_SET
    jit_api
        DESTINATION
            ${CMAKE_INSTALL_LIBEXECDIR}/tt-metalium/tt_metal # FIXME: fix the include paths for jit_build
        COMPONENT metalium-runtime
)
