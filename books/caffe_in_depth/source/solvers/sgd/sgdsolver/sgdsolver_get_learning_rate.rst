##############################################################################
(sgdsolver) GetLearningRate
##############################################################################

Learning Rate Decay Policy:

- fixed: ``fixed = base_lr``
- step: ``step = base_lr * gamma ^ (floor( iter/step ))``
- exp: ``exp = base_lr * gamma ^ iter``
- inv: ``inv = base_lr * (1 + gamma * iter) ^ (- power)``
- multistep: similar to ``step``, but it allows non uniform steps defined by stepvalue
- poly: ``poly = base_lr (1 - iter/max_iter) ^ (power)``, a polynomial decay
- sigmoid: ``sigmoid = base_lr( 1 / ( 1 + exp(-gamma * (iter - stepsize))))``, a sigmoid decay

Parameters:

- ``base_lr``: solver param
- ``max_iter``: solver param
- ``gamma``: solver param
- ``step``: solver param
- ``stepvalue``: solver param
- ``power``: solver param
- ``iter``: the current iteration

==============================================================================
(rate) fixed
==============================================================================

- input: ``base_lr``
- output: rate
- calculations:
    - rate = base_lr

::

    rate = this->param_.base_lr()

==============================================================================
(rate) step
==============================================================================

- inputs:
    - base_lr
    - gamma
    - iter
    - step
- output:
    - rate
- calculations:
    - rate = base_lr * gamma ^ (floor ( iter / step )) 

::

    this->current_step_ = this->iter_ / this->param_.stepsize();
    rate = this->param_.base_lr() * pow(this->param_.gamma(), this->current_step_);


==============================================================================
(rate) exp
==============================================================================

- inputs:
    - base_lr
    - gamma
    - iter
- output:
    - rate
- calculations:
    - rate = base_lr * gamma ^ iter

::

    rate = this->param_.base_lr() * pow(this->param_.gamma(), this->iter_);


==============================================================================
(rate) inv
==============================================================================


==============================================================================
(rate) multistep
==============================================================================


==============================================================================
(rate) poly
==============================================================================


==============================================================================
(rate) sigmoid
==============================================================================

