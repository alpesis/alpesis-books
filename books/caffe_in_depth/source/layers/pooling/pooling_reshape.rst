##############################################################################
(pooling) Reshape
##############################################################################

1. get image channels, height, and width

::

    channels_ = bottom[0]->channels();
    height_ = bottom[0]->height();
    width_ = bottom[0]->width();

2. get kernel_h_ and kernel_w_

::

    if global_pooling: 
        kernel_h_ = bottom[0]->height
        kernel_w_ = bottom[0]->width

3. get pooled_height_ and pooled_width_

::

    pooled_height_ = static_cast<int>(ceil(static_cast<float>(height_ + 2 * pad_h_ - kernel_h_) / stride_h_)) + 1;
    pooled_width_ = static_cast<int>(ceil(static_cast<float>(width_ + 2 * pad_w_ - kernel_w_) / stride_w_)) + 1;

    if (pad_h_ || pad_w_)
    {
       // If we have padding, ensure that the last pooling starts strictly
       // inside the image (instead of at the padding); otherwise clip the last.
       if ((pooled_height_ - 1) * stride_h_ >= height_ + pad_h_)
       {
         --pooled_height_;
       }

       if ((pooled_width_ - 1) * stride_w_ >= width_ + pad_w_)
       {
         --pooled_width_;
       }
       CHECK_LT((pooled_height_ - 1) * stride_h_, height_ + pad_h_);
       CHECK_LT((pooled_width_ - 1) * stride_w_, width_ + pad_w_);
    }


4. get top.shape

::

    top[0]->Reshape(bottom[0]->num(),
                    channels_,
                    pooled_height_,
                    pooled_width_);

     if (top.size() > 1)
     {
       top[1]->ReshapeLike(*top[0]);
     }


5. if max pooling, get max_idx_

::

    // If max pooling, we will initialize the vector index part.
    if (this->layer_param_.pooling_param().pool() == PoolingParameter_PoolMethod_MAX && top.size() == 1)
    {
        max_idx_.Reshape(bottom[0]->num(),
                         channels_,
                         pooled_height_,
                         pooled_width_);
    }


6. if stochastic pooling, get rand_idx_

::

     // If stochastic pooling, we will initialize the random index part.
     if (this->layer_param_.pooling_param().pool() == PoolingParameter_PoolMethod_STOCHASTIC)
     {
        rand_idx_.Reshape(bottom[0]->num(),
                          channels_,
                          pooled_height_,
                          pooled_width_);
     }

