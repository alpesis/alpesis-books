##############################################################################
(sgdsolver) ApplyUpdate
##############################################################################

Steps:

::

    1. rate = GetLearningRate()

    2. if (display && iter % display == 0): logging

    3. ClipGradients()

    4. for params_ids:
        - Normalize(param_id)
        - Regularize(param_id)
        - ComputeUpdateValue(param_id, rate) 

    5. net_->Update()

Source Codes:

::

    CHECK(Caffe::root_solver());

    Dtype rate = GetLearningRate();

    if (this->param_.display() && this->iter_ % this->param_.display() == 0) 
    {
        LOG(INFO) << "Iteration " << this->iter_ << ", lr = " << rate;
    }

    ClipGradients();

    for (int param_id = 0; param_id < this->net_->learnable_params().size(); ++param_id) 
    { 
        Normalize(param_id);
        Regularize(param_id);
        ComputeUpdateValue(param_id, rate);
    }

    this->net_->Update();

