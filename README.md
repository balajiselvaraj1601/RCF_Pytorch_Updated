# RCF_Pytorch_Updated
Pytorch re-implementation of RCF. This PyTorch implementation is based on awesome implementation by XuanyiLi.
There were minor performance fixes made in this implementation.

Major Changes are as follows
1. Fixing the weights of the upsampling layer [based on work by Weijian Xu for PyTorch HED  implementation]
   
        Initializing the value of upsampling layer each time during training would make the training less stable. In this repo, the upsampling layer weights get initialized only during the model initialization step.

2. Setting the parameters of weight_deconv5 as shown below [based on Original RCF Implementation]
        
        # self.weight_deconv5 =  make_bilinear_weights(32, 1).cuda()   
        self.weight_deconv5 =  make_bilinear_weights(16, 1).cuda()
        
3. Setting the initial value of fuse layer as 0.20 [based on Original RCF Implementation]
4. Modified the crop layer [based on BDCN implementation]

The above mentioned changes boosted the overall ODS and OIS compared to implementation by XuanyiLi.



### Results on BSDS Dataset alone

| Method |ODS F-score on BSDS500 dataset |OIS F-score on BSDS500 dataset|
|:---|:---:|:---:|
|ours| 0.795 | 0.812 |
|meteorshowers[4]| 0.790 | 0.809 |
| Reference[1]| 0.798 | 0.815  |



### Sample Output

From Left to Right  : 

        1. Original image
        
        2. Image output from meteorshowers
        
        3. Image output from our repo

<p float="left">
  <img src="/results/43051.jpg" width="250" />
  <img src="/results/43051_meteorshowers.png" width="250" />
  <img src="/results/43051_ours.png" width="250" />
</p>


<p float="left">
  <img src="/results/8068.jpg" width="250" />
  <img src="/results/8068_meteorshowers.png" width="250" />
  <img src="/results/8068_ours.png" width="250" />
</p>


As can be seen above, strong edges are detected properly with high confidence and less noise.

### Dataset
To download dataset, kindly follow the procedure as mentioned in [1]


### Usage

#### Edge Detection Pipeline

To download the vgg16 pretrained backbone model. please click

https://drive.google.com/file/d/1lUhPKKj-BSOH7yQL0mOIavvrUbjydPp5/view?usp=sharing

To train a RCF model on BSDS500:

        python train_RCF.py

If you have multiple GPUs on your machine, you can also run the multi-GPU version training:

        CUDA_VISIBLE_DEVICES=0,1 python train_multi_gpu.py --num_gpus 2

After training, to evaluate:

        We have used the pipeline suggested by [2]. It's very fast compared to the approach suggested in [1]
        The results would vary by about 0.001 due to usage of approach [2], due to different parameter values used in NMS estimation


### To do
To reach the ODS and OIS score mentioned in the paper


### Notes
Run for 10 epochs. That would be sufficient. You could get the mentioned score within 3-8 epochs itself.

The log file of our training  [sgd-0-log.txt] can be viewed to check whether your training is proceeding in similar way.

### Acknowledgements:

[1] <a href="https://github.com/yun-liu/rcf">Original RCF Implementation</a> 

[2] <a href="https://github.com/xwjabc/hed">HED Implementation</a> 

[3] <a href="https://github.com/pkuCactus/BDCN">BDCN Implementation</a> 

[4] <a href="https://github.com/meteorshowers/RCF-pytorch">RCF PyTorch Implementation</a>



### Citations

If you are using the code/model/data provided here in a publication, please consider citing original paper:

    @article{liu2019richer,
      title={Richer Convolutional Features for Edge Detection},
      author={Liu, Yun and Cheng, Ming-Ming and Hu, Xiaowei and Bian, Jia-Wang and Zhang, Le and Bai, Xiang and Tang, Jinhui},
      journal={IEEE Transactions on Pattern Analysis and Machine Intelligence (TPAMI)},
      volume={41},
      number={8},
      pages={1939--1946},
      year={2019},
      publisher={IEEE}
    }

    @inproceedings{liu2017richer,
      title={Richer Convolutional Features for Edge Detection},
      author={Liu, Yun and Cheng, Ming-Ming and Hu, Xiaowei and Wang, Kai and Bai, Xiang},
      booktitle={IEEE conference on computer vision and pattern recognition (CVPR)},
      pages={3000--3009},
      year={2017}
    }
