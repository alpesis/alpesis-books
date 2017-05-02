##############################################################################
(pooling) Forward_cpu
##############################################################################

1. get bottom and top data

::

    const Dtype* bottom_data = bottom[0]->cpu_data();
    Dtype* top_data = top[0]->mutable_cpu_data();

2. get top_count

::

    const int top_count = top[0]->count();


3. process mask

::

    // We'll output the mask to top[1] if it's of size >1.
    const bool use_top_mask = top.size() > 1;
    int* mask = NULL;  // suppress warnings about uninitalized variables
    Dtype* top_mask = NULL;

4. process max pooling

::

    // initialization
    if use_top_mask:
        top_mask = top[1]->mutable_cpu_data();
        caffe_set(top_count, Dtype(-1), top_mask);
    else:
        mask = max_idx_.mutable_cpu_data();
        caffe_set(top_count, -1, mask);

    caffe_set(top_count, Dtype(-FLT_MAX), top_data);


5. process average pooling

