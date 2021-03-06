# GradSLAM-RGB-D-Completion

 Hello there! This is the Github repository for RGB-D Image Completion leveraging Multi-View gradients from GradSLAM. This project is based on leveraging pixel level gradient information backpropagated by GradSLAM to optimize noise/semanticly corrupted RGB-D Images. [GradSLAM](https://gradslam.github.io/) is a "Automagically" differentiable SLAM framework which opens up a lot of new doors for learnt systems in 3D spatial information based Scene Perception, Robotic Localization and Mapping systems. This Github repo consists of preliminary experiments and findings on use of gradslam based gradient information for 3D computer vision.
  
## Experiments

This section consists of the results and visualizations of experiments on GradSLAM based optimization of noise/semantic corrupted RGB-D image. A total of five different initialization experiments are performed to gain insights on optimization performance for both RGB and 16-Bit Depth Images. The five different corrupted RGB-D intializations performed are:
1) Constant Value RGB-D Image
2) Uniform Noise RGB-D Image
3) Semantic Corruption of RGB-D Image
4) Noise Addition to RGB-D Image
5) Salt & Pepper Noise RGB Image with Gt Depth

Across all the experiments, two loss optimization techniques are used. The first one (parallel optimization) being simulatneously optimizing both colors, depth of RGB-D Image with a chamfer distance calculated across the colors and points of the pointclouds of ground-truth and corrupted 3D reconstructions. The second one (seq optimization) is sequentially optimizing color and depth of RGB-D Image with their respective chamfer distance between ground-truth and noisy pointclouds. On observing the Structural Similarity Index (SSIM) and Mean Square Error (MSE) values of the recovered RGB-D Images it was found that the sequential loss optimization yields better performance. An analysis on the reason of this behaviour is provided in the Insights & Discussion section.

### 1) Constant Value RGB-D Image

In this Experiment, the corrupted RGB-D Image is intialized with a constant value. Here, the RGB Image is intialized with the color 'grey' (color of the backgorund wall in the groundtruth ICL-NUIR frames) and the 16-bit Depth Image is initialized with a value of 10,015.

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/constant.png" />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_constant.png" />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/cv_d.gif" />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/cv_r.gif"  />

### 2) Uniform Noise Image

In this Experiment, the corrupted RGB-D Image is initialized with a uniform gaussian noise image.

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/uniform.png" />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_uniform.png" />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/un_d.gif"  />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/un_r.gif" />

### 3) Semantic Corruption of Gt RGB-D Image

In this Experiment, the corrupted RGB-D image is intialized by semantically modifying the ground-truth RGB-D Image. This experiment gives us a sense of how the gradients would help in optimizing an adversarially attacked RGB-D Image. For this experiment, the segmentation mask is generated by using the ESANet (SUNRGB-D) model from TUI_NCR.

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/semantic.png" />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_semantic.png"  />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sem_d.gif"  />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sem_r.gif"  />

### 4) Slight Noise addition to Gt RGB-D Image

In this Experiment, the corrupted RGB-D Image is intialized by adding gaussian noise to the ground truth image.

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/slight.png"   />

The results of the seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s_slight.png"  />

GIFs representing seq optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sn_d.gif"  />
<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/gifs/sn_r.gif"  />

### 5) Salt & Pepper Noise RGB Image with Gt Depth

In this Experiment, the corrupted RGB Image is a Salt & Pepper Gaussian Noise Image and the Depth Image is the ground-truth.

The results of the parallel optimization process:

<img src="https://github.com/NikV-JS/GradSLAM-RGB-D-Completion/blob/main/images/s&p.png"  />

The salt & pepper noise optimization experiment gives us insights on how noise rgb initializations are recovered. Initially noise is softened and the features of the RGB Image are recovered, then further rgb optimization process is similar to denoising of image.

## Insights & Discussion

The various experiments performed with various initialization and odometry methods provide insights into how gradslam provides multi-view rich-spatial pixel-wise gradient information for optimizing a SLAM system input. Initially, all the experiments were performed with the ground-truth 3D reconstruction having visual odometry utilizing ground-truth poses, whereas the corrupted 3D reconstruction having differentiable GradICP visual odometry. In this scenario, the recovered depth images closely resembled the above-showcased results, but they had a relative pose error. This pose error was also visible in the optimized 3D reconstruction. This relative pose change occurs due to gradient values from GradICP also updating the depth image. Since for PointFusion, the camera pose estimation and SLAM data association process are strongly interlinked, the visual odometry method was also changed to ground-truth for the corrupted 3D reconstruction, which mitigated the pose error in the optimized depth images.

Among the two-loss optimization techniques tested, across all experiments the sequential optimization turned out to be the best in terms of results for rgb recovery. For RGB optimization, the nearest neighbours point (in the ground truth pointcloud) of each point in corrupted pointcloud is found out and the color difference is used as the loss. Hence when we do sequential optimization we optimize depth initially and thereby allowing fast recovery of rgb image.

In the slight uniform noise experiment, the framework initially softens the noise. Here it can be seen that the sequential optimization process provides better depth and rgb recovery over the parallel one.

The constant value initialization experiment results show that the framework is quick to optimize the depth image. For the RGB Image, the features are quicky recovered and then color recovery takes place.

The Semantic corruption of the RGB-D Image provides a perfect look at gradslam gradients' prowess in spatial data. The gradient-based optimization successfully detects the depth adversarial part and quickly optimizes it in the first 50 iterations. For RGB optimization, it is seen that the sequential optimization provides more aggressive color recovery compared to parallel optimization, which is in line with the nature of the color loss.

The framework is successful in optimizing RGB-D Images irrespective of initialization. Among all initialization experiments, the performance in depth is similar but for rgb optimization the semantic corruption experiment proves to be the hardest of the lot, where the framework still greatly optimizes the semantic corruption.

## Conclusion

The Experimental Results and Insights gathered from the project show us that gradient information from GradSLAM is spatially rich. This information can be leveraged to develop learnt 3D Computer Vision systems that can also provide interpretable intermediate representations. The differentiable SLAM framework enables consistent representation learning and opens up exciting realms in 3D Computer Vision. This project provides an initial vision in that direction.

## Running Experiments on Colab

All the Experimental Results can be recreated in a google colab notebook or locally by the following code:
``` 
!pip install -q 'git+https://github.com/gradslam/gradslam.git' 
!git clone https://github.com/NikV-JS/GradSLAM-RGB-D-Completion.git
%cd /content/GradSLAM-RGB-D-Completion
!python main.py --save_dir='/content/' --experiment
```
Here, the --experiment flag can be 'semantic', 'constant_value', 'uniform_noise', 'slight_noise',  or 'salt_pepper'.
Also, main.py corresponds to seq optimization process and parallel_optim_main.py corresonds to parallel optimization process.

## Data & Code Archive

The complete results are present here: [Drive](https://drive.google.com/drive/folders/11nt_CYTVnaJvjiLGoz4nScv52EGAK5qL?usp=sharing)

The code and plots have been developed here: [Google Colab](https://colab.research.google.com/drive/17rzI9mFtUlNk5kKQF-Sgc5IvD8IrGSqT?usp=sharing)

## Acknowledgements

The following code repositories have been very helpful for the project:
1) [GradSLAM Documentation](https://github.com/gradslam/gradslam)
2) [Chamfer Distance](https://github.com/krrish94/chamferdist)
3) [Semantic Segmentation](https://github.com/TUI-NICR/ESANet/tree/main)

I thank the REAL Lab (MILA) for providing me an opportunity to explore the potential of leveraging multi-view gradients from GradSLAM for 3D Computer Vision.
