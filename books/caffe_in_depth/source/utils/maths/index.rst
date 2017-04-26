##############################################################################
Maths
##############################################################################

==============================================================================
Unary/Binary Functions
==============================================================================

------------------------------------------------------------------------------
Unary Functions
------------------------------------------------------------------------------

.. toctree::
   :maxdepth: 3

   maths_caffe_sqr.rst
   maths_caffe_exp.rst
   maths_caffe_log.rst
   maths_caffe_abs.rst
   maths_caffe_powx.rst


------------------------------------------------------------------------------
Binary Functions
------------------------------------------------------------------------------

.. toctree::
   :maxdepth: 3

   maths_caffe_add.rst
   maths_caffe_sub.rst
   maths_caffe_mul.rst
   maths_caffe_div.rst

==============================================================================
BLAS
==============================================================================

1. scalar-vector operations

- sscal, dscal
- scopy, dcopy
- isamax, idamax
- saxpy, daxpy
- sdot, ddot
- dasum
- dnrm2
- drot

2. matrix-vector operations

- sgemv, dgemv
- strmv, dtrmv
- strsv, dtrsv
- dgbmv
- sger, dger
- dsymv
- dtbmv
- dsyr

3. matrix-matrix operations

- sgemm, dgemm
- ssyrk, dsyrk
- strsm, dtrsm
- strmm, dtrmm
- ssymm, dsymm
- ssyr2k, dsyr2k

Precisions:

- S: real single precision
- D: real double precision
- C: complex single precision
- Z: complex double precision


------------------------------------------------------------------------------
Scalar-vector operations
------------------------------------------------------------------------------

.. toctree::
   :maxdepth: 3

   maths_caffe_axpy.rst
   maths_caffe_scal.rst
   maths_caffe_asum.rst

------------------------------------------------------------------------------
Vector-vector operations
------------------------------------------------------------------------------

.. toctree::
   :maxdepth: 3

   maths_caffe_dot.rst
   maths_caffe_axpby.rst
   
------------------------------------------------------------------------------
Matrix-vector operations
------------------------------------------------------------------------------

.. toctree::
   :maxdepth: 3

   maths_caffe_gemv.rst


------------------------------------------------------------------------------
Matrix-matrix operations
------------------------------------------------------------------------------


.. toctree::
   :maxdepth: 3

   maths_caffe_gemm.rst

------------------------------------------------------------------------------
References
------------------------------------------------------------------------------

- `Basic Linear Algebra Subprograms Library Programmer's Guide and API Reference`_

.. _`Basic Linear Algebra Subprograms Library Programmer's Guide and API Reference`: http://www.iman1.jo/iman1/images/IMAN1-User-Site-Files/Libraries/BLAS_Prog_Guide_API_v3.1.pdf


==============================================================================
Random
==============================================================================

.. toctree::
   :maxdepth: 3

   maths_caffe_rng_rand.rst
   maths_caffe_rng_uniform.rst
   maths_caffe_rng_gaussian.rst
   maths_caffe_rng_bernoulli.rst


