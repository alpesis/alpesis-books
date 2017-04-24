##############################################################################
Convolutions
##############################################################################

==============================================================================
Overview
==============================================================================

.. toctree::
   :maxdepth: 3

   overview_demo.rst
   overview_data.rst
   overview_functions.rst


==============================================================================
(layer) Convolution
==============================================================================

.. toctree::
   :maxdepth: 3

   conv_compute_output_shape.rst
   conv_forward_cpu.rst
   conv_backward_cpu.rst
   conv_forward_gpu.rst
   conv_backward_gpu.rst

==============================================================================
(layer) BaseConvolution
==============================================================================

.. toctree::
   :maxdepth: 3

   baseconv_layersetup.rst
   baseconv_reshape.rst
   baseconv_forward_cpu_gemm.rst
   baseconv_forward_cpu_bias.rst
   baseconv_backward_cpu_gemm.rst
   baseconv_backward_cpu_bias.rst
   baseconv_weight_cpu_gemm.rst
   baseconv_conv_im2col_cpu.rst
   baseconv_conv_col2im_cpu.rst

==============================================================================
(utils) IM2COL/COL2IM
==============================================================================

.. toctree::
   :maxdepth: 3

   utils_im2col_cpu.rst
   utils_im2col_nd_cpu.rst
   utils_col2im_cpu.rst
   utils_col2im_nd_cpu.rst


==============================================================================
(utils) GEMM/GEMV
==============================================================================

.. toctree::
   :maxdepth: 3

   utils_caffe_cpu_gemm.rst
   utils_caffe_cpu_gemv.rst
