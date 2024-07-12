# Installation

>The Installation step is referenced from [CenterNet](https://github.com/xingyizhou/CenterNet/blob/master/readme/INSTALL.md).
>
><br/>
>
>**Our experimental environment:** 
>
> Ubuntu 20.04,  Python 3.7.16, PyTorch 1.7.1, torchvision 0.8.2.
>
> NVIDIA 3090

<br/>

1. Create a new conda environment and activate the environment.

   ~~~powershell
   conda create --name MOC python=3.6
   conda activate MOC
   ~~~
   
2. Install pytorch1.4:

   ~~~powershell
   pip install in pytorch.org.
   - or you can use conda. 
   ~~~

   Disable cudnn batch normalization(follow [CenterNet](https://github.com/xingyizhou/pytorch-pose-hg-3d/issues/16)).
   
    For other pytorch version, you can manually open `torch/nn/functional.py` and find the line with `torch.batch_norm` and replace the `torch.backends.cudnn.enabled` with `False`. 

3. Clone this repo:

   ~~~powershell
   git clone https://github.com/NEUdeep/MOC-Detector-Pytorch1.4
   ~~~


4. Install the requirements

   ~~~powershell
   pip install -r requirments.txt -i https://mirrors.aliyun.com/pypi/simple
   ~~~

5. Compile deformable convolutional in DLA backbone follow [CenterNet](https://github.com/xingyizhou/CenterNet/blob/master/readme/INSTALL.md).
   https://github.com/MatthewHowe/DCNv2

   ~~~powershell
   cd ${MOC_ROOT}/src/network/DCNv2
   python setup.py build develop
   - or you must check with https://github.com/CharlesShang/DCNv2.git to make sure that is for you.
   ~~~

7. If you have any problems, please check in [CenterNet](https://github.com/xingyizhou/CenterNet/issues).
