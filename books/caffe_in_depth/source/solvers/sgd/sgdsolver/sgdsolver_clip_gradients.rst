##############################################################################
(sgdsolver) ClipGradients
##############################################################################


- inputs:
    - clip_graidents
    - net_params
- output:
    - net_params[i]->scale_diff(net_params)
- calculations:
    - clip_gradients:
        - clip_gradients = param.clip_gradients()
        - if clip_gradients < 0, return
    - net_params & sumsq_diff
        - net_params = net_->learnable_params()
        - for net_params: sumsq_diff += net_params[i]->sumsq_diff() 
    - l2norm_diff
        - l2norm_diff = sqrt(sumsq_diff)
        - if l2norm_diff > clip_gradients: 
            - scale_factor = clip_gradients / l2norm_diff
            - for net_params: net_params[i]->scale_diff(scale_factor)

::

    std::cout << "(SGDSolver) ClipGradients: " << std::endl;

    const Dtype clip_gradients = this->param_.clip_gradients();
    if (clip_gradients < 0) { return; }
     
    const vector<Blob<Dtype>*>& net_params = this->net_->learnable_params();
    Dtype sumsq_diff = 0;
    for (int i = 0; i < net_params.size(); ++i)
    {
       sumsq_diff += net_params[i]->sumsq_diff();
    }

    const Dtype l2norm_diff = std::sqrt(sumsq_diff);
    if (l2norm_diff > clip_gradients)
    {
        Dtype scale_factor = clip_gradients / l2norm_diff;
        LOG(INFO) << "Gradient clipping: scaling down gradients (L2 norm "
                  << l2norm_diff << " > " << clip_gradients << ") "
                  << "by scale factor " << scale_factor;
        for (int i = 0; i < net_params.size(); ++i)
        {
           net_params[i]->scale_diff(scale_factor);
        }
    }


