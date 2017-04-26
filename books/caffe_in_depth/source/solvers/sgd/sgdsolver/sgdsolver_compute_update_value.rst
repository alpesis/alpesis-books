##############################################################################
(sgdsolver) ComputeUpdateValue
##############################################################################

- inputs:
    - net_params_lr
    - momentum
- output:
    - net_params_diff
- calculations:
    - local_rate = rate * net_params_lr[param_id]
    - history_ = local_rate * diff + momentum * history_
    - net_params_diff = history_

::

    std::cout << "(SGDSolver) ComputeUpdateValue: " << std::endl;
    
    const vector<Blob<Dtype>*>& net_params = this->net_->learnable_params();
    const vector<float>& net_params_lr = this->net_->params_lr();
    
    Dtype momentum = this->param_.momentum();
    Dtype local_rate = rate * net_params_lr[param_id];


       case Caffe::CPU:
       {
         caffe_cpu_axpby(net_params[param_id]->count(),
                         local_rate,
                         net_params[param_id]->cpu_diff(),
                         momentum,
                         history_[param_id]->mutable_cpu_data());
         caffe_copy(net_params[param_id]->count(),
                    history_[param_id]->cpu_data(),
                    net_params[param_id]->mutable_cpu_diff());
         break;
       }


