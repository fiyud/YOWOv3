# YOWOv3: An Efficient and Generalized Framework for Human Action Detection and Recognition

This is an implementation of paper : [YOWOv3: An Efficient and Generalized Framework for Human Action Detection and Recognition](https://arxiv.org/abs/2408.02623).


---
## Preface 

Hello, thank you everyone for your attention to this study. If you find it valuable, please consider leaving a star, as it would greatly encourage me.

If you intend to use this repository for your own research, please consider to cite:
```latex
@misc{dang2024yowov3efficientgeneralizedframework,
      title={YOWOv3: An Efficient and Generalized Framework for Human Action Detection and Recognition}, 
      author={Duc Manh Nguyen Dang and Viet Hang Duong and Jia Ching Wang and Nhan Bui Duc},
      year={2024},
      eprint={2408.02623},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2408.02623}, 
}
```
---
## About asking question
I am very pleased that everyone has shown interest in this project. There are many questions being raised, and I am more than willing to answer them as soon as possible. However, if you have any doubts about the code or related matters, please provide me with context (config file, some samples that you couldn't detect, a checkpoint, etc.). Also, please use **English**.


---
## Structure of Instruction

In this Instruction, I will divide it into smaller sections, with each section serving a specific purpose. I will provide a summary of this Instruction structure in order right below. Please read carefully to locate the information you are looking for.

* **Preparation**: Environment setup and dataset preparation guide. 
* **Basic Usage**: Guide on using the pre-existing train, evaluate, detect, and live code in the repository.
* **Customization**: Guide on customizing datasets and components within the model.
* **Pretrained Resources**: Pretrained resources for 2D backbone, 3D backbone, and model checkpoints.
* **Limitations and Future Development**: In this section, I will outline the limitations that I have identified in this project. I did not have enough time and resources to experiment and do everything, so I will leave it for other research in the future.
* **Some Notes**: I will add some points that I think may cause difficulties or misunderstandings in using the repository. I will also update it based on the issues created.
* **References**: This section cites the repositories that I primarily relied on throughout the development of this project. These repositories have been incredibly helpful, and I am very grateful to the authors for providing them to the research community.



---
## Preparation
### Environment setup 
Clone this repository
```cpp 
git clone https://github.com/AakiraOtok/YOWOv3.git
```

Use Python 3.8 or Python 3.9, and then download the dependencies:

```powershell
pip install -r requirements.txt
```

**Note**:
On my system, I use Python 3.7 with slightly different dependencies, specifically for torch:

```python
torch==1.13.1+cu117
torchaudio==0.13.1+cu117
torchvision==0.14.1+cu117
```
However, when testing on another system, it seems that these versions have been deprecated. I have updated the requirements.txt file and tested it again on systems using Python 3.8 and Python 3.9, and everything seems to be working fine. If you encounter any errors during the environment setup, please try asking in the "issues" section. Perhaps someone has faced a similar issue and has already found a solution.


### Datasets

#### UCF101-24
- Download from (as in YOWOv2): https://drive.google.com/file/d/1Dwh90pRi7uGkH5qLRjQIFiEmMJrAog5J/view

#### AVAv2.2 
- Follow the instructions at: https://github.com/yjh0410/AVA_Dataset.
- If you find that video downloading takes too long, you can consider downloading them from my Hugging Face repository and then proceed with the remaining steps as instructed above: https://huggingface.co/datasets/manh6054/avav2.2_train_set/tree/main.


---

## Basic Usage

### About config file
The project is designed in such a way that almost every configuration can be adjusted through the config file. In the repository, I have provided two sample config files: ucf_config.yaml and ava_config.yaml for the UCF101-24 and AVAv2.2 datasets, respectively. The "Basic Usage" section will not involve extensive modifications of the config file, while the customization of the config will be covered in the "Customization" section.

### Simple command line

We have the following command template:
```powershell
python main.py --mode [mode] --config [config_file_path]
```
Or the shorthand version:

```powershell
python main.py -m [mode] -cf [config_file_path]
```

For ```[mode] = {train, eval, detect, live}``` for training, evaluation, detection (visualization on the current dataset), or live (camera usage) respectively. The```[config_file_path]``` is the path to the config file.

Example of training a model on UCF101-24:
```powershell
python main.py --mode train --config config/ucf_config.yaml
```
Or try evaluating a model on AVAv2.2:
```powershell
python main.py -m eval -cf config/ava_config.yaml
```

## Customization

Updating ...

## Pretrained Resources

All pre-trained models for backbone2D, backbone3D and model checkpoints are publicly available on [my Hugging Face repo](https://huggingface.co/manh6054/YOWOv3/tree/main).

Regarding the model checkpoints, I have consolidated them into an Excel file that looks like this:

Each cell represents a model checkpoint, displaying information such as mAP, GLOPs, and # param in order. The checkpoints are stored as folders named after the corresponding cells in the Excel file (e.g., O27, N23, ...). Each folder contains the respective config file used for training that model. Please note that both the regular checkpoint and the exponential moving average (EMA) version of the model are saved.

## Limitations and Future Development

- The architecture of YOWOv3 does not differ much from YOWOv2, although initially I had planned to try a few things. Firstly, the feature map from the 3D branch does not go through path aggregation but merges with the 2D branch and is used by the model for predictions directly. This makes the architecture look quite simple, and I believe it will have a significant impact on performance. Another thing is that in [this](https://arxiv.org/abs/2108.07755) paper, the authors propose an alternative method to replace the decoupled head called Task-aligned head, which can avoid repeating attention modules in YOWOv3 and make the model much lighter.

- The project was developed gradually through stages and gradually expanded, so there are still some areas that are not comprehensive enough. For example, evaluating on AVA v2.2 took a lot of time because I did not parallelize this process (batch_size = 1). The reason for this is that the format required by AVA v2.2 demands an additional identity for the bounding box, which I did not set up in the initial evaluation code as it was only serving experiments on UCF101-24 at that time.


## Some notes:
- In the commit history, there are commits named after the best checkpoints. The code in that commit is what I used to train the model, but it's not the config file! This is because during the training process, I opened another terminal window to experiment with a few things, so the config file changed and I didn't revert it back to the original. The original configs are saved in my notes, not in the commit files.

## References

I would like to express my sincere gratitude to the following amazing repositories/codes, which were the primary sources I heavily relied on and borrowed code from during the development of this project:

- [A neat implementation of YOLOv8 using PyTorch](https://github.com/jahongir7174/YOLOv8-pt)
- [3D CNN backbones: MobileNet/v2, ShuffleNet/v2, ResNet, ResNeXt](https://github.com/okankop/Efficient-3DCNNs)
- [Implementation of the I3D model on PyTorch](https://github.com/piergiaj/pytorch-i3d)
- [YOWO model](https://github.com/wei-tim/YOWO?tab=readme-ov-file)
- [YOWOv2 model](https://github.com/yjh0410/YOWOv2)
- [AVAv2.2 evaluation code from the organizers](https://github.com/activitynet/ActivityNet/tree/master/Evaluation)
