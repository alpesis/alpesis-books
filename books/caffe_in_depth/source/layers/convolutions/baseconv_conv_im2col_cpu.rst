##############################################################################
(base_conv) conv_im2col_cpu
##############################################################################


==============================================================================
Data
==============================================================================

- inputs:
    - (data_im) data:
    - (data_col) col_buff:
    - (channels) conv_in_channels_
    - (height) conv_input_shape_.cpu_data()[1]
    - (width) conv_input_shape_.cpu_data()[2]
    - (kernel_h) kernel_shape_.cpu_data()[0]
    - (kernel_w) kernel_shape_.cpu_data()[1]
    - (pad_h) pad_.cpu_data()[0]
    - (pad_w) pad_.cpu_data()[1]
    - (stride_h) stride_.cpu_data()[0]
    - (stride_w) stride_.cpu_data()[1]
    - (dilation_h) dilation_.cpu_data()[0]
    - (diliation_w) dilation_.cpu_data()[1]
- output:
    - (data_col) col_buff:
- conditional params:
    - force_nd_im2col_:
    - num_spatial_axes_: 


(input) data:

- shape: (C, H, W)

(input) col_buff:

- shape: (C, H, W)

::

    shape: (C, H, W)
    - shape_C: kernel_dim_ * group_ = kernel_channels * kernel_height * kernel_width * group_
    - shape_H: input/output_shape_height
    - shape_W: input_output_shape_width

    - input_shape: (height, width)


    - output_shape: (height, width) <- compute_output_shape()
    - output_h:
      - kernel_extent_h = dilation_h * (kernel_h - 1) + 1
      - output_h = (input_h + 2 * pad_h - kernel_extent_h) / stride_h + 1
    - output_w:
      - kernel_extent_w = dilation_w * (kernel_w + 1) + 1
      - output_w = (input_w + 2 * pad_w - kernel_extent_w) / stride_w + 1

    i.e. shape (C, H, W)

    inputs:
    - image: (224, 224)
    - pad: (1, 1) 
    - kernel: (3, 3, 3)
    - dilation: (1, 1)
    - stride: (1, 1)
    - group_: 1

    output: shape (27, 224, 224)
    - shape_C = 3 * 3 * 3 * 1 = 27
    - shape_H = 224
      - kernel_extent_h = 1 * (3 - 1) + 1 = 3
      - output_h = (224 + 2 * 1 - 3) / 1 + 1 = 224
    - shape_W = 224

(output) data_col:

- shape: kernel_channels * kernel_h * kernel_w * num_kernels_conv_out 
