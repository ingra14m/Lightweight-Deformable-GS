# Lightweight Deformable 3D Gaussian

## [Project page](https://ingra14m.github.io/Deformable-Gaussians/) | [Paper](https://arxiv.org/abs/2309.13101)

![Teaser image](assets/teaser.png)

This repository contains the official implementation associated with the paper "Deformable 3D Gaussians for High-Fidelity Monocular Dynamic Scene Reconstruction".



## Dataset

In our paper, we use:

- synthetic dataset from [D-NeRF](https://www.albertpumarola.com/research/D-NeRF/index.html).
- real-world dataset from [NeRF-DS](https://jokeryan.github.io/projects/nerf-ds/) and [Hyper-NeRF](https://hypernerf.github.io/).
- The dataset in the supplementary materials comes from [DeVRF](https://jia-wei-liu.github.io/DeVRF/).

We organize the datasets as follows:

```shell
├── data
│   | D-NeRF 
│     ├── hook
│     ├── standup 
│     ├── ...
│   | NeRF-DS
│     ├── as
│     ├── basin
│     ├── ...
│   | HyperNeRF
│     ├── interp
│     ├── misc
│     ├── vrig
```

> I have identified an **inconsistency in the D-NeRF's Lego dataset**. Specifically, the scenes corresponding to the training set differ from those in the test set. This discrepancy can be verified by observing the angle of the flipped Lego shovel. To meaningfully evaluate the performance of our method on this dataset, I recommend using the **validation set of the Lego dataset** as the test set. See more in [D-NeRF dataset used in Deformable-GS](https://github.com/ingra14m/Deformable-3D-Gaussians/releases/tag/v0.1-pre-released)



## Run

### Environment

```shell
git clone https://github.com/ingra14m/Lightweight-Deformable-GS --recursive
cd Lightweight-Deformable-GS

conda create -n deformable_gaussian_env python=3.8
conda activate deformable_gaussian_env

# install pytorch
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116

# install dependencies
pip install -r requirements.txt
```



### Train

**D-NeRF:**

```shell
python train.py -s path/to/your/d-nerf/dataset -m output/exp-name --eval --is_blender
```

**NeRF-DS/HyperNeRF:**

```shell
python train.py -s path/to/your/real-world/dataset -m output/exp-name --eval
```

**6DoF Transformation:**

We have also implemented the 6DoF transformation of 3D-GS, which may lead to an improvement in metrics but will reduce the speed of training and inference.

```shell
# D-NeRF
python train.py -s path/to/your/d-nerf/dataset -m output/exp-name --eval --is_blender --is_6dof

# NeRF-DS & HyperNeRF
python train.py -s path/to/your/real-world/dataset -m output/exp-name --eval --is_6dof
```



### Render & Evaluation

```shell
python render.py -m output/exp-name --mode render
python metrics.py -m output/exp-name
```

We provide several modes for rendering:

- `render`: render all the test images
- `time`: time interpolation tasks for D-NeRF dataset
- `all`: time and view synthesis tasks for D-NeRF dataset
- `view`: view synthesis tasks for D-NeRF dataset
- `original`: time and view synthesis tasks for real-world dataset



## Results

### D-NeRF



## Acknowledgments

We sincerely thank the authors of [3D-GS](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/), [D-NeRF](https://www.albertpumarola.com/research/D-NeRF/index.html), [HyperNeRF](https://hypernerf.github.io/), [NeRF-DS](https://jokeryan.github.io/projects/nerf-ds/), and [DeVRF](https://jia-wei-liu.github.io/DeVRF/), whose codes and datasets were used in our work. We thank [Zihao Wang](https://github.com/Alen-Wong) for the debugging in the early stage, preventing this work from sinking. We also thank the reviewers and AC for not being influenced by PR, and fairly evaluating our work. This work was mainly supported by ByteDance MMLab.




## BibTex

```
@article{yang2023deformable3dgs,
    title={Deformable 3D Gaussians for High-Fidelity Monocular Dynamic Scene Reconstruction},
    author={Yang, Ziyi and Gao, Xinyu and Zhou, Wen and Jiao, Shaohui and Zhang, Yuqing and Jin, Xiaogang},
    journal={arXiv preprint arXiv:2309.13101},
    year={2023}
}
```

And thanks to the authors of [3D Gaussians](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/) for their excellent code, please consider also cite this repository:

```
@Article{kerbl3Dgaussians,
      author       = {Kerbl, Bernhard and Kopanas, Georgios and Leimk{\"u}hler, Thomas and Drettakis, George},
      title        = {3D Gaussian Splatting for Real-Time Radiance Field Rendering},
      journal      = {ACM Transactions on Graphics},
      number       = {4},
      volume       = {42},
      month        = {July},
      year         = {2023},
      url          = {https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/}
}
```

