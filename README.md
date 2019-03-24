# RFBNet_master

使用RFBNet模型训练自己的voc数据，教程请阅读pdf文件，代码请下载压缩包！原作者模型地址：https://github.com/ruinmessi/RFBNet

1、环境搭建
  Installation
Install PyTorch-0.4.0 by selecting your environment on the website and running the appropriate command.
Clone this repository. This repository is mainly based on ssd.pytorch and Chainer-ssd, a huge thank to them.
Note: We currently only support PyTorch-0.4.0 and Python 3+.
Compile the nms and coco tools:
./make.sh
Note: Check you GPU architecture support in utils/build.py, line 131. Default is:

2、数据准备
Datasets
To make things easy, we provide simple VOC and COCO dataset loader that inherits torch.utils.data.Dataset making it fully compatible with the torchvision.datasets API.

3、voc的windows版本标注工具下载：https://download.csdn.net/download/yunxinan/11033039


4、Training
First download the fc-reduced VGG-16 PyTorch base network weights at: https://s3.amazonaws.com/amdegroot-models/vgg16_reducedfc.pth or from our BaiduYun Driver

MobileNet pre-trained basenet is ported from MobileNet-Caffe, which achieves slightly better accuracy rates than the original one reported in the paper, weight file is available at: https://drive.google.com/open?id=13aZSApybBDjzfGIdqN1INBlPsddxCK14 or BaiduYun Driver.

By default, we assume you have downloaded the file in the RFBNet/weights dir:

为了国内方便下载权重其他地址：https://download.csdn.net/download/yunxinan/11033019

5、训练于评估请参考我写的pdf和原作者的相关解释。

mkdir weights
cd weights
wget https://s3.amazonaws.com/amdegroot-models/vgg16_reducedfc.pth
To train RFBNet using the train script simply specify the parameters listed in train_RFB.py as a flag or manually change them.

python train_RFB.py -d VOC -v RFB_vgg -s 300 

6、Evaluation

To evaluate a trained network:
python test_RFB.py -d VOC -v RFB_vgg -s 300 --trained_model /path/to/model/weights

By default, it will directly output the mAP results on VOC2007 test or COCO minival2014. For VOC2012 test and COCO test-dev results, you can manually change the datasets in the test_RFB.py file, then save the detection results and submitted to the server.
