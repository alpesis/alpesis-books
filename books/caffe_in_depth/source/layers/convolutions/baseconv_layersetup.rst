##############################################################################
(base_conv) LayerSetUp
##############################################################################

==============================================================================
1. (conv_param) force_nd_im2col_
==============================================================================

::

    force_nd_im2col_ = conv_param.force_nd_im2col();

==============================================================================
2. (conv_param) channel_axis_
==============================================================================

::

    channel_axis_ = bottom[0]->CanonicalAxisIndex(conv_param.axis());

==============================================================================
3. (bottom[0]) num_spatial_axes_
==============================================================================

::

    first_spatial_axis = channel_axis_ + 1
    num_axes = bottom[0]->num_axes()
    num_spatial_axes_ = num_axes - first_spatial_axis
    CHECK: num_spatial_axes_ >= 0

    i.e. bottom[0].shape: (1, 3, 600, 800)

    - first_spatial_axis = 1 + 1 = 2
    - num_spatial_axes_ = 4 - 2 = 2


==============================================================================
4. (bottom[0]) bottom_dim_blob_shape
==============================================================================

::

    bottom_dim_blob_shape(1, num_spatial_axes_ + 1)


==============================================================================
5. (bottom[0]) spatial_dim_blob_shape
==============================================================================

::

    spatial_dim_blob_shape(1, max(num_spatial_axes_, 1))


==============================================================================
6. (conv_param) kernel
==============================================================================

::

    kernel_shape_.Reshape(spatial_dim_blob_shape)
    kernel_h, kernel_w

==============================================================================
7. (conv_param) stride
==============================================================================

::

    stride_shape_.Reshape(spatial_dim_blob_shape)
    stride_h, stride_w

==============================================================================
8. (conv_param) pad
==============================================================================

::

    pad_shape_.Reshape(spatial_dim_blob_shape)
    pad_h, pad_w


==============================================================================
9. (conv_param) dilation
==============================================================================

::

    dilation_shape_.Reshape(spatial_dim_blob_shape)
    dilation_h, dilation_w


==============================================================================
10. (im2col) is_1x1_
==============================================================================

::

   // Special case: im2col is the identity for 1x1 convolution with stride 1
   // and no padding, so flag for skipping the buffer and transformation.
   is_1x1_ = true;
   for (int i = 0; i < num_spatial_axes_; ++i)
   {
     is_1x1_ &= kernel_shape_data[i] == 1 && stride_data[i] == 1 && pad_data[i] == 0;
     if (!is_1x1_) { break; }
   }


==============================================================================
11. (bottom[0]) channels_
==============================================================================

::

    channels_ = bottom[0]->shape(channel_axis_)


==============================================================================
12. (conv_param) num_output_
==============================================================================

::

    num_output_ = this->layer_param_.convolution_param().num_output()
    CHECK: num_output_ > 0

==============================================================================
13. (conv_param) group_
==============================================================================


::

    group_ = this->layer_param_.convolution_param().group();
    CHECK_EQ(channels_ % group_, 0);
    CHECK_EQ(num_output_ % group_, 0) << "Number of output should be multiples of group.";


==============================================================================
14. (conv) conv_out_channels_, conv_in_channels_
==============================================================================

::

   if (reverse_dimensions())
   {
     conv_out_channels_ = channels_;
     conv_in_channels_ = num_output_;
   }
   else
   {
     conv_out_channels_ = num_output_;
     conv_in_channels_ = channels_;
   }


==============================================================================
15. (conv) blobs_, weights, bias
==============================================================================


blobs_:

- blobs_[0]: weights
- blobs_[1]: biases

weights:

- weight_shape[0] = conv_out_channels_
- weight_shape[1] = conv_in_channels_ / group_
- data: (conv_param) weight_filler
- kernel_dim_ = weights.count(1)
- weight_offset_ = conv_out_channels_ * kernel_dim_ / group_

biases:

- bias_term_: (conv_param)
- bias_shape: (bias_term_, num_output_)
- data: (conv_param) bias_filler

::

  // Handle the parameters: weights and biases.
  // - blobs_[0] holds the filter weights
  // - blobs_[1] holds the biases (optional)
  vector<int> weight_shape(2);
  weight_shape[0] = conv_out_channels_;
  weight_shape[1] = conv_in_channels_ / group_;
  for (int i = 0; i < num_spatial_axes_; ++i) 
  {
    weight_shape.push_back(kernel_shape_data[i]);
  }

  bias_term_ = this->layer_param_.convolution_param().bias_term();
  vector<int> bias_shape(bias_term_, num_output_);

  if (this->blobs_.size() > 0) 
  {
    CHECK_EQ(1 + bias_term_, this->blobs_.size()) << "Incorrect number of weight blobs.";
    if (weight_shape != this->blobs_[0]->shape()) 
    {
      Blob<Dtype> weight_shaped_blob(weight_shape);
      LOG(FATAL) << "Incorrect weight shape: expected shape "
                 << weight_shaped_blob.shape_string() << "; instead, shape was "
                 << this->blobs_[0]->shape_string();
    }

    if (bias_term_ && bias_shape != this->blobs_[1]->shape()) 
    {
      Blob<Dtype> bias_shaped_blob(bias_shape);
      LOG(FATAL) << "Incorrect bias shape: expected shape "
                 << bias_shaped_blob.shape_string() << "; instead, shape was "
                 << this->blobs_[1]->shape_string();
    }
    LOG(INFO) << "Skipping parameter initialization";
  } 
  else 
  {
    if (bias_term_) 
    {
      this->blobs_.resize(2);
    } 
    else 
    {
      this->blobs_.resize(1);
    }

    // Initialize and fill the weights:
    // output channels x input channels per-group x kernel height x kernel width
    this->blobs_[0].reset(new Blob<Dtype>(weight_shape));
    shared_ptr<Filler<Dtype> > weight_filler(GetFiller<Dtype>(
        this->layer_param_.convolution_param().weight_filler()));
    weight_filler->Fill(this->blobs_[0].get());

    // If necessary, initialize and fill the biases.
    if (bias_term_) 
    {
      this->blobs_[1].reset(new Blob<Dtype>(bias_shape));
      shared_ptr<Filler<Dtype> > bias_filler(GetFiller<Dtype>(
          this->layer_param_.convolution_param().bias_filler()));
      bias_filler->Fill(this->blobs_[1].get());
    }
  }
  kernel_dim_ = this->blobs_[0]->count(1);
  weight_offset_ = conv_out_channels_ * kernel_dim_ / group_;
  // Propagate gradients to the parameters (as directed by backward pass).

==============================================================================
16. (conv) param_propagate_down_
==============================================================================

::

  this->param_propagate_down_.resize(this->blobs_.size(), true);


