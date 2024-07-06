# IOSSAM: Label Efficient Multi-View Prompt-Driven Tooth Segmentation
This is the repository of our MICCAI 2024 paper "IOSSAM: Label Efficient Multi-View Prompt-Driven Tooth Segmentation".
It is a SAM-based label efficient tooth segmentation method for digital dentistry.

## Installation
Step1: install Segment Anything.

Please follow the official instructions [here](https://github.com/facebookresearch/segment-anything).

Step2: install other important packages.

Here are some important packages.
```
python>=3.7
pytorch>=1.13
open3d==0.16.0
opencv_contrib_python==4.7.0.72
opencv_python==4.8.0.74
pymeshlab==2022.2.post2
trimesh==3.21.2
bpy==4.1.0
```
The more detailed dependencies can be checked in the ```requirments.txt```.

## Getting Started
### Dataset
Please download the [Teeth3ds](https://github.com/abenhamadou/3DTeethSeg22_challenge) dataset. We use the lower jaw data in our work and follow the official data split.

### Preprocessing Dataset
Step1: Processing the original 3d teeth dataset.
```shell
python prepare_3d_data.py
```

Step2: Preparing multi-view 2d color images and depth images. A tooth model is rendered into 2d color images and depth images. We use 'Blender' to automate this process. The blender script can be seen in ```prepare_2d_data.py```

Step3: Preparing weakly-supervised data for the training of 'Occlusal Prompter'. First, we project 3d masks as masks of 2d rendered images. Then, 2d bounding box labels are obtained from 2d masks. Finally, we convert the detection dataset formate into 'CoCo' formate.
```shell
python prepare_2d_GT.py
python prepare_detection_data.py
```

### 'Occlusal Prompter' Training and Inference
Please following instructions [here](https://github.com/facebookresearch/detr) to train the query-based detector. We develop a new script to inference all the testing data and save their predicted semantic bounding box.
```shell
python occlusal_prompter_inference.py
```

### Getting Final 3D Semantic Segmentation Results
The script first use SAM to segment the occlusal view based on prompts obtained from 'Occlusal Prompter'. Then, the 'Dental Crown Prompter' provides dental crown prompts to segment the dental crown view. Finally, the 'view-aware Label Diffusion' module merges all 2d segmentation to 3d semantic segmentation.
```shell
python main.py
```

## Citation
If you find IOSSAM useful to your research, please cite our work:

## Acknowledgements
IOSSAM is inspirited by the following repos: [Segment Anything](https://github.com/facebookresearch/segment-anything),[Segment Anything 3D](https://github.com/Pointcept/SegmentAnything3D).