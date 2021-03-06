# Copyright 2011 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

########################################################################
# Include python install macros
########################################################################
include(GrPython)
if(NOT PYTHONINTERP_FOUND)
    return()
endif()

########################################################################
# Install python sources
########################################################################
GR_PYTHON_INSTALL(
    FILES
    __init__.py
    plot_waterfall.py
    waterfall_sink.py
    pdu_trim_uvector.py
    radio_source.py
    iq_only_receiver.py
    pmt_to_pdu.py
    threshold_printer.py DESTINATION ${GR_PYTHON_DIR}/starcoder
)

########################################################################
# Handle the unit tests
########################################################################
include(GrTest)

########################################################################
# Generate protobuf source
########################################################################
get_filename_component(hw_proto "${CMAKE_CURRENT_SOURCE_DIR}/../../api/starcoder.proto" ABSOLUTE)
protobuf_generate_python(hw_proto_src "${hw_proto}")
add_custom_target(myTarget ALL DEPENDS ${hw_proto_src})

set(GR_TEST_TARGET_DEPS gnuradio-starcoder myTarget)
set(GR_TEST_PYTHON_DIRS ${CMAKE_BINARY_DIR}/swig)
GR_ADD_TEST(qa_command_source ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_command_source.py)
GR_ADD_TEST(qa_ar2300_source ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_ar2300_source.py)
GR_ADD_TEST(qa_complex_to_msg_c ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_complex_to_msg_c.py)
GR_ADD_TEST(qa_waterfall_heatmap ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_waterfall_heatmap.py)
GR_ADD_TEST(qa_waterfall_plotter ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_waterfall_plotter.py)
GR_ADD_TEST(qa_waterfall_sink ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_waterfall_sink.py)
configure_file(
${CMAKE_CURRENT_SOURCE_DIR}/test_waterfall.png
${CMAKE_CURRENT_BINARY_DIR}/test_waterfall.png
COPYONLY
)
GR_ADD_TEST(qa_pdu_trim_uvector ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_pdu_trim_uvector.py)
GR_ADD_TEST(qa_golay_decoder ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_golay_decoder.py)
GR_ADD_TEST(qa_cw_to_symbol ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_cw_to_symbol.py)
GR_ADD_TEST(qa_morse_decoder ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/qa_morse_decoder.py)
