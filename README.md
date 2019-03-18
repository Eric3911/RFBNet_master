# RFBNet_master

使用RFBNet模型训练自己的voc数据，教程请阅读pdf文件，代码请下载压缩包！

1、环境搭建
  Installation
Install PyTorch-0.4.0 by selecting your environment on the website and running the appropriate command.
Clone this repository. This repository is mainly based on ssd.pytorch and Chainer-ssd, a huge thank to them.
Note: We currently only support PyTorch-0.4.0 and Python 3+.
Compile the nms and coco tools:
./make.sh
Note: Check you GPU architecture support in utils/build.py, line 131. Default is:

'nvcc': ['-arch=sm_52',
Then download the dataset by following the instructions below and install opencv.
conda install opencv
Note: For training, we currently support VOC and COCO.

2、数据准备
Datasets
To make things easy, we provide simple VOC and COCO dataset loader that inherits torch.utils.data.Dataset making it fully compatible with the torchvision.datasets API.

VOC Dataset
Download VOC2007 trainval & test
  specify a directory for dataset to be downloaded into, else default is ~/data/
sh data/scripts/VOC2007.sh # <directory>
Download VOC2012 trainval
  specify a directory for dataset to be downloaded into, else default is ~/data/
sh data/scripts/VOC2012.sh # <directory>
COCO Dataset
Install the MS COCO dataset at /path/to/coco from official website, default is ~/data/COCO. Following the instructions to prepare minival2014 and valminusminival2014 annotations. All label files (.json) should be under the COCO/annotations/ folder. It should have this basic structure

$COCO/
$COCO/cache/
$COCO/annotations/
$COCO/images/
$COCO/images/test2015/
$COCO/images/train2014/
$COCO/images/val2014/
UPDATE: The current COCO dataset has released new train2017 and val2017 sets which are just new splits of the same image sets.

3、voc的windows版本标注工具下载：https://download.csdn.net/download/yunxinan/11033039

Training
First download the fc-reduced VGG-16 PyTorch base network weights at: https://s3.amazonaws.com/amdegroot-models/vgg16_reducedfc.pth or from our BaiduYun Driver

MobileNet pre-trained basenet is ported from MobileNet-Caffe, which achieves slightly better accuracy rates than the original one reported in the paper, weight file is available at: https://drive.google.com/open?id=13aZSApybBDjzfGIdqN1INBlPsddxCK14 or BaiduYun Driver.

By default, we assume you have downloaded the file in the RFBNet/weights dir:

4、下载权重：https://download.csdn.net/download/yunxinan/11033019
5、训练于评估请参考我写的pdf和原作者的相关解释。

mkdir weights
cd weights
wget https://s3.amazonaws.com/amdegroot-models/vgg16_reducedfc.pth
To train RFBNet using the train script simply specify the parameters listed in train_RFB.py as a flag or manually change them.
python train_RFB.py -d VOC -v RFB_vgg -s 300 
Note:
-d: choose datasets, VOC or COCO.
-v: choose backbone version, RFB_VGG, RFB_E_VGG or RFB_mobile.
-s: image size, 300 or 512.
You can pick-up training from a checkpoint by specifying the path as one of the training parameters (again, see train_RFB.py for options)
If you want to reproduce the results in the paper, the VOC model should be trained about 240 epoches while the COCO version need 130 epoches.
Evaluation
To evaluate a trained network:

python test_RFB.py -d VOC -v RFB_vgg -s 300 --trained_model /path/to/model/weights
By default, it will directly output the mAP results on VOC2007 test or COCO minival2014. For VOC2012 test and COCO test-dev results, you can manually change the datasets in the test_RFB.py file, then save the detection results and submitted to the server.
