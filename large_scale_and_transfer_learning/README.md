# Edge-augmented Graph Transformer (PyTorch)

## Requirements

* `python >= 3.7`
* `pytorch >= 1.6.0`
* `numpy >= 1.18.4`
* `numba >= 0.50.1`
* `ogb >= 1.3.2`
* `rdkit>=2019.03.1`
* `yaml >= 5.3.1`

## Run Training and Evaluations

You can specify the training/prediction/evaluation configurations by creating a `yaml` config file and also by passing a series of `yaml` readable arguments. (Any additional config passed as argument willl override the config specified in the file.)

* To run training: ```python run_training.py [config_file.yaml] ['config1: value1'] ['config2: value2'] ...```
* To make predictions: ```python make_predictions.py [config_file.yaml] ['config1: value1'] ['config2: value2'] ...```
* To perform evaluations: ```python do_evaluations.py [config_file.yaml] ['config1: value1'] ['config2: value2'] ...```

Config files for the results can be found in the configs directory. Examples:
```
python run_training.py configs/pcqm4m/egt_47m.yaml
python run_training.py 'scheme: pcqm4m' 'model_height: 6'
python make_predictions.py configs/pcqm4m/egt_47m.yaml 'evaluate_on: ["val"]'
```

### More About Training

Once the training is started a model folder will be created in the *models* directory, under the specified dataset name. This folder will contain a copy of the input config file, for the convenience of resuming training/evaluation. Also, it will contain a config.yaml which will contain all configs, including unspecified default values, used for the training. Training will be checkpointed per epoch. In the case of any interruption, you can resume training by running the *run_training.py* with the config.yaml file again.

### Configs
There many different configurations. The only **required** configuration is `scheme`, which specifies the training scheme. If the other configurations are not specified, a default value will be assumed for them. Here are some of the commonly used configurations:

`scheme`: pcqm4m/pcqm4mv2/molpcba/mohiv.

`dataset_path`: Where the downloaded OGB datasets will be saved.

`model_name`: Serves as an identifier for the model, also specifies default path of the model directory, weight files etc.

`save_path`: The training process will create a model directory containing the logs, checkpoints, configs, model summary and predictions/evaluations. By default it creates a folder at *models/<dataset_name>* but it can be changed via this config.

`cache_dir`: During first time of training/evaluation the data will be cached. Default path is *cache_data/<dataset_name>*. But it can be changed via this config.

`distributed`: In a multi-gpu setting you can set it to True, for distributed training. Note that, the batch size should also be adjusted accordingly.

`batch_size`: Batch size. In case of distributed training it is the local batch size. So, the total batch size = batch_size x number of available gpus.

`num_epochs`: Maximum Number of epochs.

`max_lr`: Maximum learning rate.

`min_lr`: Minimum learning rate.

`lr_warmup_steps`: Initial linear learning rate warmup steps.

`lr_total_steps`: Total number of gradient updates to be performed, including linear warmup and cosine decay.

`model_height`: The number of layers *L*.

`node_width`: The dimensionality of the node channels *d_h*.

`edge_width`: The dimensionality of the edge channels *d_e*.

`num_heads`: The number of attention heads. Default is 8.

`node_ffn_multiplier`: FFN multiplier for node channels.

`edge_ffn_multiplier`: FFN multiplier for edge channels.

`virtual_nodes`: number of virtual nodes. 0 (default) would result in global average pooling being used instead of virtual nodes.

`upto_hop`: Clipping value of the input distance matrix.

`attn_dropout`: Dropout rate for the attention matrix.

`node_dropout`: Dropout rate for the node channel's MHA and FFN blocks.

`edge_dropout`: Dropout rate for the edge channel's MHA and FFN blocks.

`sel_svd_features`: Rank of the SVD encodings *r*.

`svd_calculated_dim` : Number of left and right singular vectors calculated and cached for svd encodings.

`svd_output_dim` : Number of left and right singular vectors used as svd encodings.

`svd_random_neg` : Whether to randomly flip the signs of the singular vectors. Default - true.

`pretrained_weights_file` : Used to specify the learned weights of an already trained model.

## Python Environment

The Anaconda environment in which the experiments were conducted is specified in the `environment.yml` file.
