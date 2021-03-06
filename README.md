<p align="center">
<a href="https://layer6.ai/"><img src="https://github.com/layer6ai-labs/DropoutNet/blob/master/logs/logobox.jpg" width="180"></a>
</p>

## NeurIPS'19 Guided Similarity Separation for Image Retrieval

Authors: [Chundi Liu](http://www.cs.toronto.edu/~chundiliu), [Guangwei Yu](http://www.cs.toronto.edu/~guangweiyu), [Cheng Chang](https://scholar.google.com/citations?user=X7oyRLIAAAAJ&hl=en), Himanshu Rai, Junwei Ma, [Satya Krishna Gorti](http://www.cs.toronto.edu/~satyag/), [Maksims Volkovs](http://www.cs.toronto.edu/~mvolkovs)  
[[paper](http://www.cs.toronto.edu/~mvolkovs/NIPS2019_GSS.pdf)][[poster]](http://www.cs.toronto.edu/~chundiliu/data/NeurIPS_GSS%20Poster.pdf)[[slides]](http://www.cs.toronto.edu/~chundiliu/data/GSS%20Slides.m4v)[[video]](https://slideslive.com/38921753/track-3-session-4)

## Prerequisites and Environment

* tensorflow-gpu 1.13.1
* numpy 1.16.0


All experiments were conducted on a 20-core Intel(R) Xeon(R) CPU E5-2630 v4 @2.20GHz and NVIDIA V100 GPU with 32GB GPU memory.

**Note:** INSTRE dataset contains almost 30k images so the training phase requires ~19GB GPU memory. If your GPU doesn't have sufficient memory, please remove the `--gpu-id` argument and the model will train on the CPU. Training on CPU is approximately 10x slower but converges to the same result.

## Dataset

We provide all three datasets used in the paper, together with generated GeM descriptors and RANSAC verification results for each dataset. To run the model, download the data from [here](https://s3.amazonaws.com/public.layer6.ai/GSS/GSS.tar.gz)
and extract it to a directory, referred to as `$DATA_DIR`. This directory should have the following structure:
```
$DATA_DIR
  ├─ datasets
  │   ├─ instre
  |   |   ├─ gnd_instre.mat
  |   |   ├─ instre_gem.mat
  |   |   └─ instre_siamac.mat
  │   ├─ roxford5k
  |   |   ├─ gnd_roxford5k.mat
  |   |   └─ gnd_roxford5k.pkl
  |   └─ rparis6k
  |   |   ├─ gnd_rparis6k.mat
  |   |   └─ gnd_rparis6k.pkl
  ├─ features				
  │   ├─ instre_gem_index_ms_lw.npy
  │   ├─ instre_gem_query_ms_lw.npy
  │   ├─ roxford5k_resnet_rsfm120k_gem.mat
  │   └─ rparis6k_resnet_rsfm120k_gem.mat
  └─ graphs
      ├─ instre_index_ransac_graph.npy
      ├─ instre_query_ransac_graph.npy
      ├─ roxford5k_index_ransac_graph.npy
      ├─ roxford5k_query_ransac_graph.npy
      ├─ rparis6k_index_ransac_graph.npy
      └─ rparis6k_query_ransac_graph.npy
```
Provide `$DATA_DIR` as the argument to `--data-path` when running `train.py`.

## Descriptors

For all the experiments, we use image descriptors generated by the pre-trained [GeM](https://arxiv.org/abs/1711.02512) model. The code and the pre-trained weights can be found in the author's [official github repository](https://github.com/filipradenovic/cnnimageretrieval-pytorch).

## Examples
------

### rOxford medium 77.8
```
python train.py --data-path $DATA_DIR --dataset roxford5k --num-layers 2 --k 5 --kq 5 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode descriptor --beta-percentile 98
```

### rOxford medium ransac 80.6
```
python train.py --data-path $DATA_DIR --dataset roxford5k --num-layers 2 --k 5 --kq 10 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode ransac --beta-percentile 98
```

### rOxford hard 57.5
```
python train.py --data-path $DATA_DIR --dataset roxford5k --num-layers 2 --k 5 --kq 5 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode descriptor --report-hard --beta-percentile 98
```

### rOxford hard ransac 64.7
```
python train.py --data-path $DATA_DIR --dataset roxford5k --num-layers 2 --k 5 --kq 10 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode ransac --report-hard --beta-percentile 98
```

### rParis medium 92.4
```
python train.py --data-path $DATA_DIR --dataset rparis6k --num-layers 2 --k 5 --kq 15 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode descriptor --beta-percentile 98
```

### rParis medium ransac 93.4
```
python train.py --data-path $DATA_DIR --dataset rparis6k --num-layers 2 --k 5 --kq 25 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode ransac --beta-percentile 98
```

### rParis hard 83.5
```
python train.py --data-path $DATA_DIR --dataset rparis6k --num-layers 2 --k 5 --kq 15 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode descriptor --report-hard --beta-percentile 98
```

### rParis hard ransac 85.3
```
python train.py --data-path $DATA_DIR --dataset rparis6k --num-layers 2 --k 5 --kq 25 --epoch 200 --lr 0.0001 --gpu-id 0 --graph-mode ransac --report-hard --beta-percentile 98
```

### INSTRE 89.2
```
python train.py --data-path $DATA_DIR --dataset instre --num-layers 2 --k 10 --kq 10 --epoch 500 --lr 0.0001 --graph-mode descriptor --beta-percentile 98
```

### INSTRE ransac 92.4
```
python train.py --data-path $DATA_DIR --dataset instre --num-layers 2 --k 50 --kq 20 --epoch 500 --lr 0.0001 --graph-mode ransac --beta-percentile 98
``` 

## Citation

If you find this code useful in your research, please cite the following paper:

  
    @inproceedings{liu2019guided,
      title={Guided Similarity Separation for Image Retrieval},
      author={Chundi Liu, Guangwei Yu, Cheng Chang, Himanshu Rai, Junwei Ma, Satya Krishna Gorti, Maksims Volkovs},
      booktitle={NeurIPS},
      year={2019}
    }


