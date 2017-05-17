##############################################################################
VGG
##############################################################################

==============================================================================
Model
==============================================================================

::

    00 input

    01 conv                           3x3/1,64        64x224x224
    02 activation       relu                          64x224x224
    03 pool             max           2x2/2           64x112x112
    04 conv                           3x3/1,128       128x112x112
    05 activation       relu          2x2/2           128x112x112
    06 pool             max           2x2/2           128x56x56 

    07 conv                           3x3/1,256       256x56x56
    08 activation       relu                          256x56x56
    09 conv                           3x3/1,256       256x56x56
    10 activation       relu                          256x56x56
    11 pool             max           2x2/2           256x28x28

    12 conv                           3x3/1,512       512x28x28
    13 activation       relu                          512x28x28
    14 conv                           3x3/1,512       512x28x28
    15 activation       relu                          512x28x28
    16 pool             max           2x2/2           512x14x14

    17 conv                           3x3/1,512       512x14x14
    18 activation       relu                          512x14x14
    19 conv                           3x3/1,512       512x14x14
    20 activation       relu                          512x14x14
    21 pool             max           2x2/2           512x7x7

    22 flatten                                        25088
    23 fc                             4096            4096
    24 activation       relu                          4096
    25 dropout                                        4096
    26 fc                             4096            4096
    27 activation       relu                          4096
    28 dropout                                        4096

    29 fc                             2               2
    30 softmax
