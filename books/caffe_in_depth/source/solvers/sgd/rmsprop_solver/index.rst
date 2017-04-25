##############################################################################
RMSPropSolver
##############################################################################

.. toctree::
   :maxdepth: 3

::

 31      231   // RMSProp decay value
 32  232   // MeanSquare(t) = rms_decay*MeanSquare(t-1) + (1-rms_decay)*SquareGradient(t)
 33  233   optional float rms_decay = 38;
                                             
