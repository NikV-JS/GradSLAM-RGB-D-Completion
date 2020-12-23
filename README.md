# GradSLAM-RGB-D-Completion

 Hello there! This is the Github repository for RGB-D Image Completion leveraging Multi-View gradients from GradSLAM. This project is based on leveraging pixel level gradient information backpropagated by GradSLAM to optimize noise/semanticly corrupted RGB-D Images. [GradSLAM](https://gradslam.github.io/) is a "Automagically" differentiable SLAM framework which opens up a lot of new doors for learnt systems in 3D spatial information based Scene Perception, Robotic Localization and Mapping systems. This Github repo consists of preliminary experiments and findings on use of gradslam based gradient information for 3D computer vision.
  
## Experiments

This section consists of the results and visualizations of experiments on GradSLAM based optimization of noise/semantic corrupted RGB-D image. A total of five different initialization experiments are performed to gain insights on optimization performance for both RGB and 16-Bit Depth Images. The five different corrupted RGB-D intializations performed are:
1) Constant Value RGB-D Image
2) Uniform Noise RGB-D Image
3) Semantic Adversarial attack on RGB-D Image
4) Noise Addition to RGB-D Image
5) Salt & Pepper Noise RGB Image with Gt Depth

Across all the experiments, two loss optimization techniques are used. The first one (parallel optimization) being simulatneously optimizing both colors, depth of RGB-D Image with a chamfer distance calculated across the colors and points of the pointclouds of ground-truth and corrupted 3D reconstructions. The second one (seq optimization) is sequentially optimizing color and depth of RGB-D Image with their respective chamfer distance between ground-truth and noisy pointclouds. On observing the Structural Similarity Index (SSIM) and Mean Square Error (MSE) values of the recovered RGB-D Images it was found that the sequential loss optimization yields better performance. An analysis on the reason of this behaviour is provided in the Insights & Discussion section.

### Constant Value RGB-D Image

In this Initialization, the corrupted RGB-D Image is intialized with a constant value. Here, the RGB Image is intialized with the color 'grey' (color of the backgorund wall in the groundtruth ICL-NUIR frames) and the 16-bit Depth Image is initialized with a value of 10,015.

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/constant.png" />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_constant.png" />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/cv_d.gif" />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/cv_r.gif"  />

### Uniform Noise Image

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/uniform.png" />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_uniform.png" />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/un_d.gif"  />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/un_r.gif" />

### Semantic Adversarial attack on Gt RGB-D Image

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/semantic.png" />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_semantic.png"  />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sem_d.gif"  />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sem_r.gif"  />

3D Reconstruction before optimization:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/sem.png" />

3D Reconstruction after optimization:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/recon_sem.png"  />

### Slight Noise addition to Gt RGB-D Image

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/slight.png"   />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_slight.png"  />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sn_d.gif"  />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sn_r.gif"  />

### Salt & Pepper Noise RGB Image with Gt Depth

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s&p.png"  />


## Insights & Discussion

## Conclusion & Further Study

## Running Experiments on Colab

``` 
!pip install -q 'git+https://github.com/gradslam/gradslam.git' 
!git clone https://github.com/NikV-JS/GradSLAM-RGB-D-Completion.git
%cd /content/GradSLAM-RGB-D-Completion
!python main.py --save_dir='/content/' --experiment
```

## Data & Code Archive

## Acknowledgements
