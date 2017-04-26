##############################################################################
(sgdsolver) Regularize
##############################################################################

- inputs:
    - regularization_type

L2:

- inputs:
- output:
- calculations:
      - weight_decay = param_.weight_decay()
      - net_params_weight_decay = this->net_->params_weight_decay()
      - local_decay = weight_decay * net_params_weight_decay[param_id]
      - diff = local_decay * data + diff

L1:

- inputs:
- output:
- calculations:
    - 

::


