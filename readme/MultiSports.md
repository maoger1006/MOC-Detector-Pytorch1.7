# MultiSports

## Dataset
MultiSports Dataset contains 66 fine-grained action categories from four different sports, selected from 247 competition records. 

Firstly, download the dataset and groundtruths from [here](https://huggingface.co/datasets/MCG-NJU/MultiSports) We need to convert these 25-fps videos to frames which have the same format as JHMDB for the convenience of training and evaluation. 

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
```powershell
python train.py --K 7 --exp_id Train_K7_rgb_coco_multi_s1 --rgb_model ../experiment/MultiSports/rgb_model --batch_size 2 --master_batch 2 --lr 5e-4 --gpus 0 --num_workers 1 --num_epochs 1 --lr_step 6,8 --dataset multisports --split 1
```


