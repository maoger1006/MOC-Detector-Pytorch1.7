# MultiSports

## Dataset
MultiSports Dataset contains 66 fine-grained action categories from four different sports, selected from 247 competition records. 

Firstly, download the dataset and groundtruths from [here](https://huggingface.co/datasets/MCG-NJU/MultiSports) We need to convert these 25-fps videos to frames which have the same format as JHMDB for the convenience of training and evaluation. The frames are resized to 50%, and *resolution* and the bounding box values in the *gttubes* are modified as well. 

```powershell
python3 process_videos.py
```
<br/>

```shell
${MOC_ROOT}
|-- data
`-- |-- MultiSports
    `-- |-- Frames
    `-- |-- FlowBrox04(optional)
    `-- |-- MultiSports-GT.pkl
```

   
## Train 
MultiSports dataset is very large, only basketball subclass is used for my scenario. Before training and evaluation, you need to filter them out in `base_dataset.py`, `normal_inference.py`, `ACT.py`.   

Width and length of images are resized, so the bounding box values need to be modified when reading in the `ACT.py`, `base_dataset.py`
```powershell
python train.py --K 7 --exp_id Train_K7_rgb_coco_multi_s1 --rgb_model ../experiment/MultiSports/rgb_model --batch_size 2 --master_batch 2 --lr 5e-4 --gpus 0 --num_workers 1 --num_epochs 1 --lr_step 6,8 --dataset multisports --split 1
```
```powershell
python train.py --K 7 --exp_id Train_K7_rgb_coco_multi_s1 --rgb_model ../experiment/Multisports_0718/rgb_model --batch_size 2 --master_batch 2 --lr 5e-4 --gpus 0 --num_workers 1 --num_epochs 2 --lr_step 6,8 --dataset multisports --split 1 --load_model ../experiment/MultiSports/rgb__model/model_last.pth --start_epoch 1
```

## Inference

we just trained rgb model. 
```powershell
python det.py --task normal --K 7 --gpus 0 --batch_size 1 --master_batch 1 --num_workers 2 --rgb_model ../experiment/Multisports_0718/rgb_model/model_last.pth  --inference_dir ../data0/basketball_test_1
```

```powershell
python det.py --task normal --K 7 --gpus 0 --batch_size 1 --master_batch 1 --num_workers 2 --rgb_model ../experiment/MultiSports/rgb_model/model_last.pth --inference_dir ~data/mmy/MOC/data0/basketball_test --flip_test --ninput 5 --dataset multisports 
```

## Evaluation

'ACT.py', in the frameAP, the input bbox coordinates are resized as well.  
In the `normal_inference.py`

```powershell
python ACT.py --task BuildTubes --K 11 --inference_dir ../data0/basketball_test_3_epoch4
```

For video mAP,  build tubes first:
```powershell
python ACT.py --task BuildTubes --K 11 --inference_dir ../data0/basketball_test_3_epoch4
```

Then, compute video mAP:
```powershell
python ACT.py --task videoAP --K 7 --th 0.2 --inference_dir $INFERENCE_DIR
```

I only trained RGB model for Multisports, so in the evaluation, just use rgb_model, otherwise will occur weights and bias mismatch problem.


