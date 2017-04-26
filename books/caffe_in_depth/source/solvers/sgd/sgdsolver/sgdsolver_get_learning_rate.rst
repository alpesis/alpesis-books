##############################################################################
(sgdsolver) GetLearningRate
##############################################################################

Learning Rate Decay Policy:

- fixed: ``fixed = base_lr``
- step: ``step = base_lr * gamma ^ (floor( iter/step ))``
- exp: ``exp = base_lr * gamma ^ iter``
- inv: ``inv = base_lr * (1 + gamma * iter) ^ (- power)``
- multistep: similar to ``step``, but it allows non uniform steps defined by stepvalue
- poly: ``poly = base_lr * (1 - iter/max_iter) ^ (power)``, a polynomial decay
- sigmoid: ``sigmoid = base_lr * ( 1 / ( 1 + exp(-gamma * (iter - stepsize))))``, a sigmoid decay

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

- inputs:
    - base_lr
    - gamma
    - power
    - iter
- output:
    - rate
- calculations:
    - rate = base_lr * (1 + gamma * iter) ^ (- power)


For example:

::

    - inputs:
        - base_lr: 0.01
        - gamma: 0.0001
        - power: 0.75
        - iter: 0
    - output:
        - rate
    - calculations:
        - rate = 0.01 * (1 + 0.0001 * 0) ^ (- .075) = 0.01

 
Source codes:

::

    rate = this->param_.base_lr() * pow(Dtype(1) + this->param_.gamma() * this->iter_, - this->param_.power());


==============================================================================
(rate) multistep
==============================================================================

- inputs:
    - base_lr
    - gamma
    - stepvalue
    - iter
    - current_step
- output:
    - rate
- calculations:
    - if (current_step < stepvalue && iter >= stepvalue(current_step)): current_step++
    - rate = base_lr * gamma ^ (floor( current_step ))

::

     if (this->current_step_ < this->param_.stepvalue_size() &&
         this->iter_ >= this->param_.stepvalue(this->current_step_))
     {
       this->current_step_++;
       LOG(INFO) << "MultiStep Status: Iteration " <<
       this->iter_ << ", step = " << this->current_step_;
     }
     rate = this->param_.base_lr() * pow(this->param_.gamma(), this->current_step_);


==============================================================================
(rate) poly
==============================================================================

- inputs:
    - base_lr
    - power
    - max_iter
    - iter
- output:
    - rate
- calculations:
    - rate = base_lr * (1 - iter/max_iter) ^ (power)

::

    rate = this->param_.base_lr() * pow(Dtype(1.) - (Dtype(this->iter_) / Dtype(this->param_.max_iter())), this->param_.power());


==============================================================================
(rate) sigmoid
==============================================================================

- inputs:
    - base_lr
    - gamma
    - stepsize
    - iter
- output:
    - rate
- calculations:
    - rate = base_lr * ( 1 / ( 1 + exp(-gamma * (iter - stepsize)))) 

::

    rate = this->param_.base_lr() * (Dtype(1.) / (Dtype(1.) + exp(-this->param_.gamma() * (Dtype(this->iter_) - Dtype(this->param_.stepsize())))));

