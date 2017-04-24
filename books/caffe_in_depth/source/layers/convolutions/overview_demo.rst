##############################################################################
Demo
##############################################################################

==============================================================================
1. Forward
==============================================================================

transformation
~~~~~~~~~~~~~~~~~

- (im2col) data_im -> (im2col) -> col_buff


formula
~~~~~~~~~~~~~~~~~

(gemm) C := alpha * A * B + beta * C

- (weights: C = A * B) weights * col_buff = top_data
- (bias: C = A * B + C) bias * bias_multiplier_ + top_data = top_data


shapes
~~~~~~~~~~~~~~~~~

- data_im: (C, H, W)
- col_buff: (kernel_channels * kernel_h * kernel_w, output_h * output_w)
- weights: (output_channels_ / group_, kernel_channels * kernel_h * kernel_w)
- bias: (num_output_, 1)
- bias_multiplier_: (1, output_h * output_w)
- top_data: (num_output_, output_h * output_w)


------------------------------------------------------------------------------
1.1. (forward) im2col
------------------------------------------------------------------------------

- inputs:
    - data_im: (C, H, W)
    - pad: (h, w)
    - kernel: (h, w)
    - stride: (h, w)
    - dilation: (h, w)
    - data_col: (kernel_dim_, output_h * output_w)

- output:
    - col_buff: (kernel_dim_, output_h * output_w)

- calculations:
    - output_shape: data_col(kernel_dim_, output_h * output_w)
        - kernel_dim_ = channels * kernel_h * kernel_w
        - output_h = (height + 2 * pad_h - (dilation_h * (kernel_h - 1) + 1)) / stride_h + 1
        - output_w = (width + 2 * pad_w - (dilation_w * (kernel_w - 1) + 1)) / stride_w + 1 
    - data_im -> data_col
        - loop (TODO)

------------------------------------------------------------------------------
1.2. (forward) weights
------------------------------------------------------------------------------

- inputs:
    - (weights) weights + weight_offset_ * g (conv_out_channels_ / group_, kernel_dim_)
    - (col_buff) col_buff + col_offset_ * g (kernel_dim_, output_h * output_w)

- output:
    - (output) output + output_offset_ * g (conv_out_channels_ / group_, conv_out_spatial_dim_)

- calculations:
    - shapes:
        - weights: (conv_out_channels_ / group_, kernel_dim_)
        - col_buff: (kernel_dim_, output_h * output_w)
        - output: (conv_out_channels_ / group_, conv_out_spatial_dim_)
        - calculations:
            - offsets:
                - weight_offset_ = conv_out_channels_ / group_ * kernel_dim_
                - col_offset_ = kernel_dim_ * output_h * output_w
                - output_offset_ = conv_out_channels_ /group_ * conv_out_spatial_dim_
            - dimensions:
                - conv_out_channels_:
                - kernel_dim_ = channels * kernel_h * kernel_w
                - output_h = (height + 2 * pad_h - (dilation_h * (kernel_h - 1) + 1)) / stride_h + 1
                - output_w = (width + 2 * pad_w - (dilation_w * (kernel_w - 1) + 1)) / stride_w + 1
                - conv_out_spatial_dim_ = output_h * output_w
    - gemm:
        - output + output_offset_ * g = (weights + weight_offset_ * g) * (col_buff + col_offset_ * g)


------------------------------------------------------------------------------
1.3. (forward) bias
------------------------------------------------------------------------------

- inputs:
    - biases: (num_output_, 1)
    - bias_multiplier_: (1, output_h * output_w)
    - output: (num_output_, output_h * output_w)

- output:
    - output: (num_output_, output_h * output_w)

- calculations:
    - (gemm) output = bias * bias_multiplier_ + output 


==============================================================================
2. Backward
==============================================================================
