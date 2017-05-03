##############################################################################
(layer) Reshape
##############################################################################

1. check dimensions:

::

    const int axis = bottom[0]->CanonicalAxisIndex(this->layer_param_.inner_product_param().axis());
    const int new_K = bottom[0]->count(axis);
    CHECK_EQ(K_, new_K) << "Input size incompatible with inner product parameters.";

2. get M_

::

    M_ = bottom[0]->count(0, axis);

3. process top.shape

::

    vector<int> top_shape = bottom[0]->shape();
    top_shape.resize(axis + 1);
    top_shape[axis] = N_;
    top[0]->Reshape(top_shape);


4. process bias_multiplier

::

    vector<int> bias_shape(1, M_);
    bias_multiplier_.Reshape(bias_shape);
    caffe_set(M_, Dtype(1), bias_multiplier_.mutable_cpu_data());
 
