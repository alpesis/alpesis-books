##############################################################################
Data
##############################################################################

==============================================================================
Blob
==============================================================================

blobs

- blobs_[0]: weights
- blobs_[1]: bias
- bottom
- top

blobs: size

- bottom.size()
- top.size()

blobs: shape, dimensions, spatial dimensions

- bottom/top_shape_: (channels, height, width)
- bottom/top_dim_: channels * height * width
- bottom/top/conv_spatial_dim_: height * width

==============================================================================
(proto) ConvolutionParameter
==============================================================================



1. Outputs:

=========== ======== =========== ====== ======== =====================================
Constraint    Type    Variable    No.    Default  Description
=========== ======== =========== ====== ======== =====================================
 optional    uint32   num_output   1              the number of outputs for the layer
=========== ======== =========== ====== ======== =====================================

2. Forward/Backward:

2.1. Weights:

=========== ================= =============== ====== ======== =====================================
Constraint    Type             Variable        No.    Default  Description
=========== ================= =============== ====== ======== =====================================
 optional    FillerParameter   weight_filler   7 
 optional    FillerParameter   bias_filler     8 
 optional    bool              bias_term       2      True     whether to have bias terms 
=========== ================= =============== ====== ======== =====================================

2.2. Convolution:

=========== ================= =============== ====== ======== =====================================
Constraint    Type             Variable        No.    Default  Description
=========== ================= =============== ====== ======== =====================================
 repeated    uint32            pad             3      0 
 repeated    uint32            kernel_size     4  
 repeated    uint32            stride          6      1 
 repeated    uint32            dilation        18     1  
 optional    uint32            pad_h           9      0         for 2D only 
 optional    uint32            pad_w           10     0         for 2D only 
 optional    uint32            kernel_h        11    
 optional    uint32            kernel_w        12 
 optional    uint32            stride_h        13 
 optional    uint32            stride_w        14 
 optional    uint32            group           5      1         the group size for group conv 
=========== ================= =============== ====== ======== =====================================


2.3. im2col/col2im:

=========== ================= ================ ====== ======== =====================================
Constraint    Type             Variable        No.    Default  Description
=========== ================= ================ ====== ======== =====================================
 optional    int32             axis            16     1 
 optional    bool              force_nd_im2col 17     False 
=========== ================= ================ ====== ======== =====================================

3. Engine:

=========== ================= ================ ====== ==============================================
Constraint    Type             Variable        No.    Default  
=========== ================= ================ ====== ==============================================
 optional    Engine            engine           15     default, caffe, cudnn 
=========== ================= ================ ====== ==============================================


==============================================================================
(layer) ConvolutionParameters
==============================================================================


=========== =========================== ===================== ==============
Constraint   Type                        Variable              Default
=========== =========================== ===================== ==============
public       virtual inline const char*  type()                Convolution
protected    virtual inline bool         reverse_dimensions()  False
=========== =========================== ===================== ==============
          

==============================================================================
(layer) BaseConvolutionParameters
==============================================================================

1. public blobs

========== =================== ============================== ========
Constraint  Type                Variable                      Default
========== =================== ============================== ========
public     virtual inline int  MinBottomBlobs() const         1
public     virtual inline int  MinTopBlobs() const            1
public     virtual inline bool EqualNumBottomTopBlobs() const True
========== =================== ============================== ========

2. config params for LayerSetUp

========== =================== ============================== ======== =================
Constraint  Type                Variable                      Default   Remark
========== =================== ============================== ======== =================
protected   Blob<int>           kernel_shape_                          conv_param
protected   Blob<int>           stride_                                conv_param
protected   Blob<int>           pad_                                   conv_param
protected   Blob<int>           dilation_                              conv_param
protected   int                 channel_axis_                          conv_param
protected   int                 group_                        1        conv_param / gemm
protected   int                 num_output_                            conv_param / bias
protected   bool                force_nd_im2col_                       conv_param
protected   bool                is_1x1_        
protected   bool                bias_term_
protected   int                 num_
protected   int                 channels_                              output_channels
protected   int                 num_spatial_axes_             2
protected   int                 weight_offset_                         gemm
private     int                 conv_out_channels_
private     int                 conv_in_channels_
private     int                 kernel_dim_
========== =================== ============================== ======== =================

3. interim params for Reshape

========== =================== ============================== ======== =======
Constraint  Type                Variable                      Default  Remark
========== =================== ============================== ======== =======
protected   Blob<int>           conv_input_shape_
protected   vector<int>         col_buffer_shape_
protected   const vector<int>*  bottom_shape_
protected   int                 bottom_dim_
protected   int                 top_dim_
protected   int                 out_spatial_dim_                       bias
private     int                 num_kernels_im2col_                   
private     int                 num_kernels_col2im_
private     int                 conv_out_spatial_dim_
private     int                 col_offset_                            gemm
private     int                 output_offset_ 
private     Blob<Dtype>         col_buffer_
private     Blob<Dtype>         bias_multiplier_
========== =================== ============================== ======== =======


4. input/output params

========== =================== ============================== ======== =======================
Constraint  Type                Variable                      Default   Remark
========== =================== ============================== ======== =======================
protected   inline int          input_shape
protected   virtual bool        reverse_dimensions             0
protected   vector<int>         output_shape_                           compute_output_shape
========== =================== ============================== ======== =======================
