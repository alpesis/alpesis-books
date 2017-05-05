##############################################################################
(layer) Reshape
##############################################################################

1. softmax_axis_ = axis

2. top.shape <- bottom.shape

3. get sum_multiplier.shape: (1, axis->num_axis) with value 1

::

    sum_multiplier_: shape with (1, C)

4. get outer_num_ and inner_num_

::

    outer_num_ = bottom[0]->count(0, softmax_axis_);
    inner_num_ = bottom[0]->count(softmax_axis_ + 1);

5. get scale_.shape

::

    scale_.dims = bottom.dims 
    scale_dims[softmax_axis_] = 1
    scale_.reshape(scale_dims)
