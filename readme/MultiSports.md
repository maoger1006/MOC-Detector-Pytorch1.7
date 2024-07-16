# MultiSports Dataset

MultiSports Dataset contains 66 fine-grained action categories from four different sports, selected from 247 competition records. 

Firstly, download the dataset and groundtruths from [here](https://huggingface.co/datasets/MCG-NJU/MultiSports) We need to convert these 25-fps videos to frames which have the same format as JHMDB for the convenience of training and evaluation. 

```powershell
python3 process_videos.py
```

