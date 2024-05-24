# Lightweight Deformable 3D Gaussian

## [Project page](https://ingra14m.github.io/Deformable-Gaussians/) | [Paper](https://arxiv.org/abs/2309.13101) | [Vanilla Code](https://github.com/ingra14m/Deformable-3D-Gaussians) 

![Teaser image](assets/teaser.png)

This repository serves as an enhancement project for the paper 'Deformable 3D Gaussians for High-Fidelity Monocular Dynamic Scene Reconstruction'. By implementing a densification trick, we can achieve a halved number of Gaussians and higher FPS without compromising rendering quality.



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

|          | PSNR  |  SSIM  | LPIPS(VGG) | FPS  | Mem(MB) |  Num.  |
| -------- | :---: | :----: | :--------: | :--: | :-----: | :----: |
| bouncing | 40.85 | 0.9952 |   0.0091   |  80  |  23.90  | 101031 |
| hell     | 41.39 | 0.9866 |   0.0247   | 204  |  6.65   | 27704  |
| hook     | 37.17 | 0.9859 |   0.0158   |  68  |  23.15  | 97876  |
| jump     | 37.68 | 0.9894 |   0.0135   | 128  |  11.87  | 49986  |
| mutant   | 43.32 | 0.9948 |   0.0055   |  64  |  27.5   | 116276 |
| standup  | 44.18 | 0.9946 |   0.0076   | 129  |  11.15  | 47141  |
| trex     | 37.76 | 0.9929 |   0.0099   |  47  |  34.7   | 146707 |
| average  | 40.19 | 0.9913 |   0.0123   | 103  |  19.84  | 83817  |




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

