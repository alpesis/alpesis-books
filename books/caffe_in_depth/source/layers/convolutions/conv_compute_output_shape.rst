##############################################################################
(conv) compute_output_shape
##############################################################################

Calculations:

- inputs:
    - input_dim: (height, width)
    - kernel: (height, width)
    - pad: (height, width)
    - stride: (height, width)
    - dilation: (height, width)
- outputs:
    - output_dim: (height, width)
- calculations:
    - kernel_extent = dilation * (kernel - 1) + 1
    - output_dim = (input_dim + 2 * pad - kernel_extent) / stride + 1

Extensions:

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
