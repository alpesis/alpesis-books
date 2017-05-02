##############################################################################
(pooling) LayerSetUp
##############################################################################

1. get pooling_params

2. get kernel_h_ and kernel_w_:

::

    if global_pooling:
        kernel_h_ = bottom[0].height
        kernel_w_ = bottom[0].width
    elif kernel_size:
        kernel_h_ = kernel_size.h
        kernel_w_ = kernel_size.w
    elif kernel_h and kernel_w:
        kernel_h_ = kernel_h
        kernel_w_ = kernel_w

3. get pad_h_ and pad_w_

::

    if pad:
        pad_h_ = pad.h
        pad_w_ = pad_w
    elif pad_h, pad_w:
        pad_h_ = pad_h
        pad_w_ = pad_w


4. get stride_h_ and stride_w

::

    if stride:
        stride_h_ = stride.h
        stride_w_ = stride.w
    elif stride_h, stride_w:
        stride_h_ = stride_h
        stride_w_ = stride_w

5. check global_pooling

::

    if global_pooling:
        pad == 0 && stride == 1

6. check pad

::

    if pad_h_ != 0 and pad_w_ != 0:
        PoolMethod == MAX or AVE 
