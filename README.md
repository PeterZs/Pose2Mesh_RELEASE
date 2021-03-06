# Pose2Mesh: Graph Convolutional Network for 3D Human Pose and Mesh Recovery from a 2D Human Pose
![pose to mesh](./asset/pose_mesh.png)
![quality results](./asset/quality_result.png)


## Introduction
This repository is the offical [Pytorch](https://pytorch.org/) implementation of [Pose2Mesh: Graph Convolutional Network for 3D Human Pose and Mesh Recovery from a 2D Human Pose (ECCV 2020)](). Below is the overall pipeline of Pose2Mesh.
![overall pipeline](./asset/pipeline.png)


## Install guidelines
- We recommend you to use an [Anaconda](https://www.anaconda.com/) virtual environment. Install [PyTorch](https://pytorch.org/) >= 1.2 according to your GPU driver and Python >= 3.7.2, and run `sh requirements.sh`. 


## Quick demo
- Download the pre-trained Pose2Mesh according to [this](#pretrained-model-weights).
- Prepare SMPL and MANO layers according to [this](#pytorch-smpl-and-mano-layer)
- Prepare a pose input, for instance, as `input.npy`. `input.npy` should contain the coordinates of 2D human joints, which follow the topology of joint sets defined [here](#start). The joint orders can be found in each `${ROOT}/data/*/dataset.py`.
- Run `python demo/run.py --gpu 0 --res 500 --input input.npy --joint_set {human36,coco,smpl,mano}`
- The outputs `demo_2Dpose.png`, `demo_mesh.png` will be saved in `${ROOT}/demo/result/`.
- To visualize the Pose2Mesh predictions on COCO validation set, prepare COCO data according to [this](#data), and run `python demo/run_coco.py --gpu 0 --res 500 --sample_num 10`.

## Results
Here I report the performance of Pose2Mesh.

![table](./asset/tab.png)

Below shows the results when the input is **groundtruth** 2D human poses.
For Human3.6M benchmark, Pose2Mesh is trained on Human3.6M.
For 3DPW benchmark, Pose2Mesh is trained on Human3.6M and COCO.

| | MPJPE | PA-MPJPE |
|:---:|:---:|:---:|
| Human36M | 51.28 mm | 35.61 mm |
| 3DPW | 63.10 mm | 35.37 mm |

We provide qualitative results on SURREAL to show that Pose2Mesh can recover 3D shape to some degree.
Please refer to the paper for more discussion.

![surreal quality results](./asset/surreal.png)
## Directory

### Root

The `${ROOT}` is described as below.

```
${ROOT} 
|-- data
|-- demo
|-- lib
|-- experiment
|-- main
|-- manopth
|-- smplpytorch
```
- `data` contains data loading codes and soft links to images and annotations directories.
- `demo` contains demo codes.
- `lib` contains kernel codes for Pose2Mesh.
- `main` contains high-level codes for training or testing the network.
- `experiment` contains the outputs of the system, whic include train logs, trained model weights, and visualized outputs.

### Data

The `data` directory structure should follow the below hierarchy.
```
${ROOT}  
|-- data  
|   |-- Human36M  
|   |   |-- images  
|   |   |-- annotations   
|   |   |-- J_regressor_h36m_correct.npy
|   |   |-- absnet_output_on_testset.json 
|   |-- MuCo  
|   |   |-- data  
|   |   |   |-- augmented_set  
|   |   |   |-- unaugmented_set  
|   |   |   |-- MuCo-3DHP.json
|   |   |   |-- smpl_param.json
|   |-- COCO  
|   |   |-- images  
|   |   |   |-- train2017  
|   |   |   |-- val2017  
|   |   |-- annotations  
|   |   |-- J_regressor_coco.npy
|   |   |-- hrnet_output_on_valset.json
|   |-- PW3D 
|   |   |-- data
|   |   |   |-- 3DPW_train.json
|   |   |   |-- 3DPW_validation.json
|   |   |   |-- 3DPW_test.json
|   |   |   |-- hrnet_output_on_testset.json
|   |   |   |-- simple_output_on_testset.json
|   |   |-- imageFiles
|   |-- AMASS
|   |   |-- data
|   |   |   |-- cmu
|   |-- SURREAL
|   |   |-- data
|   |   |   |-- train.json
|   |   |   |-- val.json
|   |   |   |-- hrnet_output_on_testset.json
|   |   |   |-- simple_output_on_testset.json
|   |   |-- images
|   |   |   |-- train
|   |   |   |-- test
|   |   |   |-- val
|   |-- FreiHAND
|   |   |-- data
|   |   |   |-- training
|   |   |   |-- evaluation
|   |   |   |-- freihand_train_coco.json
|   |   |   |-- freihand_train_data.json
|   |   |   |-- freihand_eval_coco.json
|   |   |   |-- freihand_eval_data.json
|   |   |   |-- hrnet_output_on_testset.json
|   |   |   |-- simple_output_on_testset.json
```
- Download Human3.6M parsed data and SMPL parameters [[data](https://drive.google.com/drive/folders/1kgVH-GugrLoc9XyvP6nRoaFpw3TmM5xK)][[SMPL parameters from SMPLify-X](https://drive.google.com/drive/folders/1s-yywb4zF_OOLMmw1rsYh_VZFilgSrrD)]
- Download MuCo parsed/composited data and SMPL parameters [[data](https://drive.google.com/drive/folders/1yL2ey3aWHJnh8f_nhWP--IyC9krAPsQN)][[SMPL parameters from SMPLify-X](https://drive.google.com/drive/folders/1_JrrbHZICDTe1lqi8S6D_Y1ObmrerAoU?usp=sharing)]
- Download COCO SMPL parameters [[SMPL parameters from SMPLify](https://drive.google.com/drive/folders/1X7OMEGQJOe0Tcn2GvvP1koKkq4yghIzr?usp=sharing)]  
- Download AMASS SMPL parameters [[offical site](https://amass.is.tue.mpg.de/)]
- Download SURREAL parsed data [[data](https://drive.google.com/drive/folders/1vvJXM0WYzbkjTTVAFsfXRAdyAwAhEPPh?usp=sharing)] 
- Download 3DPW parsed data [[data](https://drive.google.com/drive/folders/1fWrx0jnWzcudU6FN6QCZWefaOFSAadgR)]
- Download FreiHAND parsed data [[data](https://drive.google.com/drive/folders/1QGWu_nWi5eyrWSkPEOMyK1tAGFWRXQjC)] (`bbox` in `freihand_eval_coco.json` is from [Detectron2](https://github.com/facebookresearch/detectron2))
- All annotation files follow [MS COCO format](https://cocodataset.org/#format-data).
- If you want to add your own dataset, you have to convert it to [MS COCO format](https://cocodataset.org/#format-data).
- Images need to to be downloaded, but if needed you can download them from their offical sites.
- 2D pose detection outputs can be download here: [Human36M](https://drive.google.com/drive/folders/1iRuNa6CqoHbloJ-wFPpW6g72QmP9xZT-?usp=sharing), [COCO](https://drive.google.com/drive/folders/1x0lLocLWloN813njSTsP0K09opTcLULE?usp=sharing), [3DPW](https://drive.google.com/drive/folders/1qt5R4wMTP1FSUtSi52lUke3EQazNkqVh?usp=sharing), [SURREAL](https://drive.google.com/drive/folders/1dkHOZwaflluUCPZpbLwio9s3kN1QwHz8?usp=sharing), [FreiHAND](https://drive.google.com/drive/folders/1QfAoCNTuQKbFTIoS5p9Hm_4IRdM5xVif?usp=sharing)

If you have a problem with 'download limit' when trying to download datasets from google drive links, please try this trick.
>* Go the shared folder, which contains files you want to copy to your drive  
>* Select all the files you want to copy  
>* In the upper right corner click on three vertical dots and select “make a copy”  
>* Then, the file is copied to your personal google drive account. You can download it from your personal account.  

### Pytorch SMPL and MANO layer

- For the SMPL layer, I used [smplpytorch](https://github.com/gulvarol/smplpytorch). The repo is already included in `${ROOT}/smplpytorch`.
- Download `basicModel_f_lbs_10_207_0_v1.0.0.pkl`, `basicModel_m_lbs_10_207_0_v1.0.0.pkl`, and `basicModel_neutral_lbs_10_207_0_v1.0.0.pkl` from [here](https://drive.google.com/drive/folders/1eOWKgpdZHTmsGwXVeM2Q0PAj8axoP3tR)   to `${ROOT}/smplpytorch/smplpytorch/native/models`.
For the MANO layer, I used [manopth](https://github.com/hassony2/manopth). The repo is already included in `${ROOT}/manopth`.
Download `MANO_RIGHT.pkl` from [here](https://drive.google.com/drive/folders/1ZJHvus6iSpDEm5BwDOcR5N9T8_VJcG9u) at `${ROOT}/manopth/mano/models`.

### Experiment

The `experiment` directory will be created as below.
```
${ROOT}  
|-- experiment  
|   |-- exp_*  
|   |   |-- checkpoint  
|   |   |-- graph 
|   |   |-- vis 
```

- `experiment` contains train/test results of Pose2Mesh on various benchmark datasets.
We recommed you to create the folder as a soft link to a directory with large storage capacity.

- `exp_*` is created for each train/test command. 
The wildcard symbol refers to the time of the experiment train/test started.
Default timezone is UTC+9, but you can set to your local time.

- `checkpoint` contains the model checkpoints for each epoch. 

- `graph` contains visualized train logs of error and loss. 

- `vis` contains `*.obj` files of meshes and images with 2D human poses or human meshes. 

### Pretrained model weights
Download pretrained model weights from [here](https://drive.google.com/drive/folders/1HayITLQYf6d43ksShRYF3CU6KDKd84Kn?usp=sharing) to a corresponding directory.
```
${ROOT}  
|-- experiment  
|   |-- posenet_human36J_train_human36 
|   |-- posenet_cocoJ_train_human36_coco_muco
|   |-- posenet_smplJ_train_surreal
|   |-- posenet_manoJ_train_freihand
|   |-- pose2mesh_human36J_train_human36
|   |-- pose2mesh_cocoJ_train_human36_coco_muco
|   |-- pose2mesh_smplJ_train_surreal
|   |-- pose2mesh_manoJ_train_freihand
|   |-- posenet_human36J_gt_train_human36
|   |-- posenet_cocoJ_gt_train_human36_coco
|   |-- pose2mesh_human36J_gt_train_human36
|   |-- pose2mesh_cocoJ_gt_train_human36_coco
```

## Running Pose2Mesh

![joint set topology](./asset/joint_sets.png)
### Start
- In the `lib/core/config.py`, you can change settings of the system including a train/test dataset to use, a pre-defined joint set (one of Human36M, COCO, MANO, SMPL defined joint sets), a pre-trained PoseNet, a learning schedule, GT usage, and so on.
- Pose2Mesh uses different joint sets from Human3.6M, COCO, SMPL, MANO for Human3.6M, 3DPW, SURREAL, FreiHAND benchmarks respectively. For the COCO joint set, we manually add 'Pelvis' and 'Neck' joints by computing the middle point of 'L_Hip' and 'R_Hip', and 'L_Shoulder' and 'R_Shoulder' respectively.
- To train/test PoseNet, please refer to the [PoseNet branch]() # TO BE ADDED.

### Train

Select the config file in `${ROOT}/asset/yaml/` and train. You can change the train set and pretrained posenet. Note that the first dataset on the list should call `build_coarse_graphs` function for the graph convolution setting. Refer to the last line of `__init__` function in `${ROOT}/data/Human36M/dataset.py`. 

Run
```
python main/train.py --gpu 0,1,2,3 --cfg ./asset/yaml/*.yml
```


### Test

Select the config file in `${ROOT}/asset/yaml/` and test. You can change the pretrained model weight. To save sampled outputs to `obj` files, change `TEST.vis` value to `True` in the config file.

Run
```
python main/test.py --gpu 0,1,2,3 --cfg ./asset/yaml/*.yml
```

### Reference
```
@InProceedings{Choi_2020_ECCV_Pose2Mesh,  
author = {Choi, Hongsuk and Moon, Gyeongsik and Lee, Kyoung Mu},  
title = {Pose2Mesh: Graph Convolutional Network for 3D Human Pose and Mesh Recovery from a 2D Human Pose},  
booktitle = {European Conference on Computer Vision (ECCV)},  
year = {2020}  
}  
```
