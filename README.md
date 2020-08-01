# End-to-end Lane detection Based on Segmentation
This is a 2020 MSc Project github repository. This project study, reimplement, test and analyze the PINet model proposed by Yeongmin Ko et al.

## What we have done?
- Study and re-implement PINet for better **scalability, robustness and easier maintenance**
- Using **multi-scale training and testing technique** to test the robustness of PINet
- Test PINet with the challenging dataset - **CULane**
- Build a **real-time** [traffic lane detection interface](http://pinet.yanrucheng.com)

## Project Summary
- Supervisor: Dr. P. Luo
- Group Members: Cheng Yanru, Huang Jingxuan, Li Ling, Tong Li
- Original Paper : key points estimation and point instance segmentation approach for lane detection
- Original Paper Link : https://arxiv.org/abs/2002.06604
- Original Author : Yeongmin Ko, Jiwon Jun, Donghwuy Ko, Moongu Jeon (Gwanju Institute of Science and Technology)

- This repository is pytorch implement of the above paper. Our poposed method, PINet(Point Intance Network), combines key point estimation and point instance segmentation for lane detection. 

## Dependency
We recommend using Anaconda for easier environment mangement
- $ git clone https://github.com/yanrucheng/PINet.git
- $ cd <pinet directory>
- $ conda env create -f pinet.yml

## Dataset
### TuSimple
This code is developed on tuSimple dataset. You can download the dataset from https://github.com/TuSimple/tusimple-benchmark/issues/3. We recommand to make below structure.

    dataset
      |
      |----train_set/               # training root 
      |------|
      |------|----clips/            # video clips, 3626 clips
      |------|------|
      |------|------|----some_clip/
      |------|------|----...
      |
      |------|----label_data_0313.json      # Label data for lanes
      |------|----label_data_0531.json      # Label data for lanes
      |------|----label_data_0601.json      # Label data for lanes
      |
      |----test_set/               # testing root 
      |------|
      |------|----clips/
      |------|------|
      |------|------|----some_clip/
      |------|------|----...
      |
      |------|----test_label.json           # Test Submission Template
      |------|----test_tasks_0627.json      # Test Submission Template
            
Next, you need to change "train_root_url" and "test_root_url" to your "train_set" and "test_set" directory path in "parameters.py". For example,

```
# In "parameters.py"
line 54 : train_root_url="<tuSimple_dataset_path>/train_set/"
line 55 : test_root_url="<tuSimple_dataset_path>/test_set/"
```

Finally, you can run "fix_dataset.py", and it will generate dataset according to the number of lanes and save dataset in "dataset" directory. (We have uploaded dataset. You can use them.)

### CULane
We also support           
  
## Test
We provide trained model, and it is saved in "savefile" directory. You can run "test.py" for testing, and it has some mode like following functions 
- $ conda activate pinet
- $ python test.py -h

Output:
>>> usage: test.py [-h] [-t THRESHOLD] [-c COLOR] (-f FILENAME | -d DIRECTORY)
>>> test.py: error: one of the arguments -f/--filename -d/--directory is required

Test a single image
- $ python test.py -f test.png
The output will be saved as test_output.png

Test a directory of images
- $ python test.py -d inputs/


## Train
If you want to train from scratch, make line 13 blank in "parameters.py", and run "train.py"
```
# In "parameters.py"
line 13 : model_path = ""
```
"train.py" will save sample result images(in "test_result/"), trained model(in "savefile/"), and evaluation result for some threshold values(0.3, 0.5, 0.7). However, in the most case, around 0.8 show the best performance.

We recommand to make line 210, 211, 212 of "test.py" as comments when you train the model, because post-processing takes more time.

If you want to train from a trained model, just change following 2 lines.
```
# In "parameters.py"
line 13 : model_path = "<your model path>/"
# In "train.py"
line 54 : lane_agent.load_weights(<>, "tensor(<>)")
```
