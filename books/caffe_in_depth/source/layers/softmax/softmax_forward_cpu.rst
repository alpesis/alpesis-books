##############################################################################
(layer) Forward_cpu
##############################################################################

1. get bottom and top data

2. get scale_

3. channels

::

    channels = bottom[0]->shape(softmax_axis_)

4. dim 

::

    dim = bottom[0]->count() / outer_num_

5. top_data <- bottom_data

::

    caffe_copy(bottom[0]->count(), bottom_data, top_data)


6. calculate softmax

::

    for outer_num_: 0 -> N*C

        // initialize the scale_data
        caffe_copy(inner_num_, bottom_data + i * dim, scale_data)

        for channels:
            for inner_num_:
                scale_data[k] = std::max(scale_data[k], bottom_data[i * dim + j * inner_num_ + k])

        // subtraction
        top_data[C, H*W] = -1 * sum_multiplier[C, 1] * scale_data[1, H*W] + top_data[C, H*W]

        // expoentiation
        caffe_exp<Dtype>(dim, top_data, top_data)

        // sum after exp
        scale_data[C, H*W] = top_data[C, 1] * sum_multiplier_[1, H*W]

        // division
        for channels:
            caffe_div(inner_num_, top_data, scale_data, top_data);
            top_data += inner_num_;
