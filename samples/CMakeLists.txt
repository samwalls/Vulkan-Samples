# Copyright (c) 2019-2020, Arm Limited and Contributors
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 the "License";
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.10)

# Creates a list of the subdirectories within this folder
scan_dirs(
    DIR ${CMAKE_CURRENT_SOURCE_DIR}
    LIST SUB_DIRS)

# List of all samples
set(TOTAL_SAMPLE_ID_LIST)

# For each directory, add all the samples that exist within
function(add_sub_dirs DIRECTORY)
    scan_dirs(
        DIR ${DIRECTORY}
        LIST DIR_LIST)

    foreach(CURR_DIR ${DIR_LIST})
        if(EXISTS "${DIRECTORY}/${CURR_DIR}/CMakeLists.txt")
            add_subdirectory(${DIRECTORY}/${CURR_DIR})
            list(APPEND TOTAL_SAMPLE_ID_LIST ${CURR_DIR})
        else()
            add_sub_dirs(${DIRECTORY}/${CURR_DIR})
        endif()
        set(TOTAL_SAMPLE_ID_LIST ${TOTAL_SAMPLE_ID_LIST} PARENT_SCOPE)
    endforeach()
endfunction(add_sub_dirs)

foreach(SUB_DIR ${SUB_DIRS})
    add_sub_dirs(${CMAKE_CURRENT_SOURCE_DIR}/${SUB_DIR})
endforeach()

# Order of the sample ids
set(ORDER_LIST
    #API Samples
    "hello_triangle"
    "dynamic_uniform_buffers"
    "texture_loading"
    "hdr"
    "instancing"
    "compute_nbody"
    "terrain_tessellation"

    #Extension Samples
    "conservative_rasterization"
    "push_descriptors"
    "raytracing_basic"

    #Performance Samples
    "swapchain_images"
    "surface_rotation"
    "pipeline_cache"
    "descriptor_management"
    "render_passes"
    "render_subpasses"
    "pipeline_barriers"
    "wait_idle"
    "layout_transitions"
    "specialization_constants"
    "command_buffer_usage"
    "afbc")

# Orders the sample ids by the order list above
order_sample_list(
    INPUT ${TOTAL_SAMPLE_ID_LIST}
    OUTPUT TOTAL_SAMPLE_ID_LIST
    ORDER ${ORDER_LIST})

# Make sample list visible parent scope (required by vulkan_samples project)
set(TOTAL_SAMPLE_ID_LIST ${TOTAL_SAMPLE_ID_LIST} PARENT_SCOPE)
