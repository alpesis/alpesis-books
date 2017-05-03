##############################################################################
(layer) LayerSetUp
##############################################################################

1. get bias_term

::

    bias_term_ = bias_term

2. get num_output

::

    N_ = num_output

3. get axis

::

    axis = bottom[0]->CanonicalAxisIndex(this->layer_param_.inner_product_param().axis())

4. process K_

::

    K_ = bottom[0]->count(axis)

5. process weights and bias

::

    if blobs_.size > 0: not initialized
    else:
        if bias_term_: blobs_.size == 2
        else:          blobs_.size == 1

        // initialize weights
        get shape: (N_, K_) = (num_output, bottom(axis))
        fill weights: by weight_filler

        // initialize bias
        if bias_term_:
            get shape: (1, N_) = (1, num_output)
            fill bias: by bias_filler

6. process param_propagate_down_

::

    this->param_propagate_down_.resize(this->blobs_.size(), true)
