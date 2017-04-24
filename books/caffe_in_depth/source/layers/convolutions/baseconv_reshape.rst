##############################################################################
(base_conv) Reshape
##############################################################################


==============================================================================
1. (bottom[0]) num_axes
==============================================================================

Calculations:

- inputs: 
    - channel_axis_
    - num_spatial_axes_
- output:
    - num_axes
- calulations:
    - first_spatial_axis = channel_axis_ + 1
    - num_axes = first_spatial_axis + num_spatial_axes_

For example:

::

    i.e. bottom[0].shape: (1, 3, 600, 800)

    - inputs: 
      - channel_axis_ = 1
      - num_spatial_axes_ = 2
    - output:
      - num_axes = 4
    - calculations:
      - first_spatial_axis = channel_axis_ + 1 = 1 + 1 = 2
      - num_axes = first_spatial_axis + num_spatial_axes_ = 2 + 2 = 4

==============================================================================
2. (bottom[0]) num_
==============================================================================

Calculations:

- input: channel_axis_
- output: num_
- calculation: num_ = bottom[0]->count(0, channel_axis_)

For example:

::

   i.e. bottom[0].shape: (1, 3, 600, 800)

   - input: channel_axis_ = 1
   - output: num_ = bottom[0]->count(0, 1) = 1

==============================================================================
3. (bottom[0]) channels_
==============================================================================

Calculations:

- inputs: 
    - channels_
    - channel_axis_
- CHECK: bottom[0]->shape(channel_axis_) == channels_

For example:

::

    i.e. bottom[0].shape: (1, 3, 600, 800)

    - inputs
      - channels: 3
      - channel_axis_: 1
    - CHECK:
      - bottom[0]->shape(channel_axis_) = bottom[0]->shape(1) = 3 == 3


==============================================================================
4. (bottom[0]) shape
==============================================================================

Purpose: Check all inputs with the same shape.

Calculations:

- inputs:
    - bottom.size()
    - bottom[bottom_id]->shape()
- CHECK: bottom[0]->shape() == bottom[bottom_id]->shape()

For example:

::

    i.e. bottom: bottom[0]

    - bottom.size(): 1
    - bottom[0].shape == bottom[0].shape

    i.e. bottom: bottom[0], bottom[1]

    - bottom.size(): 2
    - bottom[0].shape == bottom[0].shape
    - bottom[0].shape == bottom[1].shape


==============================================================================
5. (bottom) bottom_shape_
==============================================================================

::

    bottom_shape_ = &bottom[0]->shape()


==============================================================================
6. (top) top_shape, output_shape_
==============================================================================

Calculations:

- inputs:
    - input_shape: (height, width)    
    - kernel: (height, width)
    - pad: (height, width)
    - stride: (height, width)
    - dilation: (height, width)
- output: 
    - top_shape_: (output_h, output_w)
- calculations:
    - kernel_extent_h = dilation_h * (kernel_h - 1) + 1
    - output_h = (input_h + 2 * pad_h - kernel_extent_h) / stride_h + 1
    - kernel_extent_w = dilation_w * (kernel_w + 1) + 1
    - output_w = (input_w + 2 * pad_w - kernel_extent_w) / stride_w + 1

For example:

::

    - inputs:
     - input_dim: (224, 224)
      - kernel: (3, 3)
      - pad: (1, 1)
      - stride: (1, 1)
      - dilation: (1, 1)

    - output: output_shape_: (224, 224)

    - calculations:
      - kernel_extent = 1 * (3 - 1) + 1 = 3
      - output_shape_ = (224 + 2 * 1 - 3) / 1 + 1 = 224



==============================================================================
7. (conv) conv_out_spatial_dim_
==============================================================================

Calculations:

- inputs:
- output:
- calculation:


For example:

::

    if (reverse_dimensions()): conv_out_spatial_dim_ = bottom[0]->count(first_spatial_axis)
    else                     : conv_out_spatial_dim_ = top[0]->count(first_spatial_axis)

    bottom[0].shape: (1, 3, 224, 244)
    top[0].shape: (1, 3, 244, 244)

    - conv_out_spatial_dim_ = height * width = 224 * 224 = 50176
   

==============================================================================
8. (conv) col_offset
==============================================================================

Calculations:

- inputs:
- output:
- calculations:

For example:

::

    col_offset_ = kernel_dim_ * conv_out_spatial_dim_;

    - kernel_dim_: channels * kernel_h * kernel_w = 3 * 3 * 3 = 27
    - conv_out_spatial_dim_ = height * width = 224 * 224 = 50176
    - col_offset_ = 27 * 50176 = 27 * 50176 = 1354752


==============================================================================
9. (conv) output_offset_
==============================================================================

Calculations:

- inputs:
    - conv_out_channels
    - conv_out_spatial_dim_
    - group_
- output:
    - output_offset_
- calculations:
    - output_offset_ = conv_out_channels_ * conv_out_spatial_dim_ / group_

For example:

::

    output_offset_ = conv_out_channels_ * conv_out_spatial_dim_ / group_

    - conv_out_channels_: 64
    - conv_out_spatial_dim_: 50176
    - group_: 1
    - output_offset_ = 64 * 50176 / 1 = 3211264


==============================================================================
10. (conv) conv_input_shape_
==============================================================================

Calculations:

- inputs:
- output:
    - conv_input_shape_: (channels, height, width)
    - conv_input_shape_data: (num_spatial_axes_, data[channel * height * width])
- calculations:

For example:


::


    - inputs:
      - bottom[0]->shape(channel_axis_, num_spatial_axes_ + 1)
      - top[0]->shape(channel_axis_, num_spatial_axes_ + 1) 
    - output:
      - conv_input_shape_: (3, 244, 244)
    - calculations:
      - conv_input_shape_:
        - bottom_dim_blob_shape: (1, num_spatial_axes_ + 1)
        - conv_input_shape_: (1, bottom_dim_blob_shape)
      - conv_input_shape_data:
        - if reverse_dimensions(): conv_input_shape_data[i] = top[0]->shape(channel_axis_ + i)
        - else                   : conv_input_shape_data[i] = bottom[0]->shape(channel_axis_ + i)

 
==============================================================================
11. (conv) col_buffer_, col_buffer_shape_
==============================================================================

Calculations:

- inputs:
- output:
- calculations:

For example:

::

    col_buffer_shape_: (channels, height, width) = (kernel_dim_ * group_, in/out_h, in/out_w)

    - col_buffer_shape_: channels = kernel_dim_ * group_
    - col_buffer_shape_: height = in/out_h = input/output_shape_h
    - col_buffer_shape_: width = in/out_w = input/output_shape_w

    for num_spatial_axes_:
        if (reverse_dimensions): input_shape(i+1)
        else                   : output_shape[i]


    col_buffer_shape_: (27, 244, 244)

==============================================================================
12. (conv) bottom_dim_, top_dim_
==============================================================================

Calculations:

- inputs:
- outputs:
- calculations:
  - bottom_dim_: channels * height * width
  - top_dim_: channels * height * width

For example:

::

    bottom_dim_ = bottom[0]->count(channel_axis_);
    top_dim_ = top[0]->count(channel_axis_);


    bottom_dim_: (3, 224, 224) = 3 * 224 * 224 = 150528
    top_dim_: (64, 224, 224) = 64 * 224 * 224 = 3211264

   
==============================================================================
13. (conv) num_kernels_im2col_, num_kernels_col2im_
==============================================================================


::

    num_kernels_im2col_ = conv_in_channels_ * conv_out_spatial_dim_;
    num_kernels_col2im_ = reverse_dimensions() ? top_dim_ : bottom_dim_;
 
    - conv_in_channels_: 3
    - conv_out_spatial_dim_: bottom/top[0].height * bottom/top[0].width = 224 * 224 = 50176
    - num_kernels_im2col_ = 3 * 50176 = 150528
    - num_kernels_col2im_ = reverse_dimensions() ? top_dim_ : bottom_dim_ = c * h * w = 3 * 224 * 224 = 150528


==============================================================================
14. (conv) bias_multiplier_
==============================================================================

Calculations:

- inputs:
- outputs:
- calculations:

For example:

::

    out_spatial_dim_ = (top[0]) height * width
    bias_multiplier_: (shape) (1, top.height * top.width) = 224 * 224 = 50176

    bias_multiplier_: [1, top.height * top.width]
      [1, 1, 1, ..., 1]

