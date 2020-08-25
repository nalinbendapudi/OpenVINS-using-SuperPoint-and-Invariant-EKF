
# OpenVINS using-SuperPoint and Invariant EKF

This project was cloned from https://github.com/robintzeng/EECS568_team_14_open_vins , which was in turn forked from https://github.com/rpng/open_vins

- Report: https://github.com/nalinbendapudi/OpenVINS-using-SuperPoint-and-Invariant-EKF/blob/master/Mobile_Robotics_Report.pdf
- Video Presentation: https://youtu.be/DY8Rjkexh38

## Introduction
This is the code repository for Team 14 of the Winter 2020 version of Mobile Robotics at University of Michigan. This project makes two changes to the Open VINS library. First, the branches ```SuperPoint``` and ```SuperPointTF``` replace the visual features with SuperPoint features ([citation for SuperPoint](https://arxiv.org/abs/1712.07629 "SuperPoint: Self-Supervised Interest Point Detection and Description")). Second, the branch ```riekf``` modifies the propagation and correction steps of the filter to use an invariant form of the Extended Kalman Filter ([citation for Invariant EKF](https://arxiv.org/abs/1904.09251 "Contact-Aided Invariant Extended Kalman Filtering for Robot State Estimation")). The invariant EKF functions rely on the code repository [invariant-ekf](https://github.com/RossHartley/invariant-ekf).

## SuperPoint
Instead of using the original ORB feature, we implement the superpoint feature extractor on both Pytorch (```SuperPoint```) and Tensorflow (```SuperPointTF```) C++ framework with ROS.

SuperPoint is a fully-convolutional neural network architecture with a VGG-like encoder. It is a learning-base feature extractor, which outperforms the handcraft feature extractor in many ways.

We use the pretrained model [here](https://github.com/rpautrat/SuperPoint/tree/master/pretrained_models) for the tensorflow version. In addition, the pretrain model for the Pytorch version are from [here](https://github.com/magicleap/SuperPointPretrainedNetwork)

For the evaluation part, we use the tensorflow version to evaluate the performance of the Superpoint. 

For the usage of the branch, please read the readme in the branch.

## Invariant Extended Kalman Filter
This project modifies the Open VINS library to an Invariant EKF for covariance propagation and correction. The original Multi-State Constraint Kalman Filter methodology for initializing and triangulating visual features is maintained. Major differences are detailed below:

The filter is composed of two stages, propagation and correction.

1. Propagation
The class method Propagation::predict_mean_discrete propagates the IMU state forward using the rotation velocity and acceleration readings from the IMU sensor (omega and a).

The class method InEKF::Propagate propagates the uncertainty in the tangent space to update the covariance matrix.

2. Correction
Visual feature 3D positions (in the global frame), are estimated using Gauss Newton optimization, according to the original MSCKF algorithm in the class method UpdaterMSCKF::update.

The estimated global feature positions are used in the invariant correction step of the class methods InEKF::CorrectLandmarks and InEKF::Correct.

  Url        = {\url{}}
}
```


The codebase is licensed under the [GNU General Public License v3 (GPL-3)](https://www.gnu.org/licenses/gpl-3.0.txt).


