##############################################################################
Data
##############################################################################

==============================================================================
(proto) PoolingParameter
==============================================================================

Conditional Params

========== ============ ================ ==== ========= ==============
Constraint  Type         Variable         No.  Default   Description
========== ============ ================ ==== ========= ==============
optional    PoolMethod   pool             1    MAX
optional    bool         global_pooling   12   False
optional    Engine       engine           11
========== ============ ================ ==== ========= ==============

Pooling Params

========== ============ ============ ==== ========= ==============
Constraint  Type         Variable     No.  Default   Description
========== ============ ============ ==== ========= ==============
optional    uint32       pad          4    0         
optional    uint32       pad_h        9    0
optional    uint32       pad_w        10   0
optional    uint32       kernel_size  2
optional    uint32       kernel_h     5
optional    uint32       kernel_w     6
optional    uint32       stride       3    1
optional    uint32       stride_h     7
optional    uint32       stride_w     8
========== ============ ============ ==== ========= ==============

