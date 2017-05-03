##############################################################################
(layer) Forward_cpu
##############################################################################

1. get bottom and top

2. get weights

3. (weights) caffe_cpu_gemm()

::

    shapes:
    - bottom: (M, K)
    - top: (M, N)
    - weights: (N, K)

    top = bottom * weights.T
    top[M, N] = bottom[M, K] * weight.T [K, N]

    - M: bottom[0]->count(0, axis)
    - K: bottom[0]->count(axis)
    - N: num_output 

    For example:
    - bottom: (N, C, H, W)
      -> axis: 1
      -> M_: N
      -> K_: C * H * W)
      -> N_: num_output

    top[N, num_output] = bottom[N, C * H * W] * weights.T [C * H * W, num_output]


4. (bias) caffe_cpu_gemm()

::

    top[M, N] = bias_multiplier_[M, 1] * bias[1, N] + top[M, N]
    top[N, num_output] = bias_multiplier_[N, 1] * bias[1, num_output] + top[N, num_output]
