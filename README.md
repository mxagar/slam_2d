# Landmark Detection & Robot Tracking (SLAM)

This repository contains a toy implementation of Simultaneous Localization and Mapping (SLAM) in 2D which uses the [Graph SLAM Algorithm](http://robots.stanford.edu/papers/thrun.graphslam.pdf). A map of the environment is created only from sensor and motion data obtained from a simulated robot moving over time and space. The location of the robot is tracked, as well as landmarks in the scene.

Th starter code was taken from a project from the [Udacity Computer Vision Nanodegree](https://www.udacity.com/course/computer-vision-nanodegree--nd891), available in its original form in the repository [udacity/P3_Implement_SLAM](https://github.com/udacity/P3_Implement_SLAM).

Table of Contents:

- [Landmark Detection & Robot Tracking (SLAM)](#landmark-detection--robot-tracking-slam)
  - [How to Use This](#how-to-use-this)
    - [Overview of Files and Contents](#overview-of-files-and-contents)
    - [Dependencies](#dependencies)
  - [Brief Notes on Simultaneous Localization and Mapping (SLAM)](#brief-notes-on-simultaneous-localization-and-mapping-slam)
  - [Improvements, Next Steps](#improvements-next-steps)
  - [Interesting Links](#interesting-links)
  - [Authorship](#authorship)

## How to Use This

First, you need to install the [dependencies](#dependencies); then, you can open and run the notebooks sequentially.

In the following sections I explain in more detail all those steps.

### Overview of Files and Contents

The project folder contains the following files:

```
.
├── 1. Robot Moving and Sensing.ipynb           # Notebook 1
├── 2. Omega and Xi, Constraints.ipynb          # Notebook 2
├── 3. Landmark Detection and Tracking.ipynb    # Notebook 3
├── Instructions.md                             # Original project instructions
├── LICENSE                                     # 
├── README.md                                   # This file
├── helpers.py                                  # 
├── images/                                     # Figures used in the notebooks
├── requirements.txt                            # Dependencies
└── robot_class.py                              # 
```

The implementation is guided by the notebooks, which either contain the necessary code or import it from different scripts (explained below).

As mentioned, first, the [dependencies](#dependencies) need to be installed. Assuming everything is set up, we can run the notebooks sequentially; they carry out the following tasks:

- [`1. Robot Moving and Sensing.ipynb`](1.\ Robot\ Moving\ and\ Sensing.ipynb): the `robot` class defined in `robot_class.py` is explained, as well as some related utilities, such as map visualization and motion handling.
- [`2. Omega and Xi, Constraints.ipynb`](2.\ Omega\ and\ Xi,\ Constraints.ipynb): the key aspects of the Graph SLAM are explained for the 1D case.
- [`3. Landmark Detection and Tracking.ipynb`](3.\ Landmark\ Detection\ and\ Tracking.ipynb): **that's where SLAM is implemented**.

On the other hand, the implementation scripts and their contents are the following:

- [`robot_class.py`](robot_class.py): `robot` class which contains the location of the robot in a 2D world; additionally, `move()` and `sense()` functions that enable interacting with the world are provided. The *landmarks* measured in the world are created here.
- [`helpers.py`](helpers.py): auxiliary functions for visualization and world setup.

### Dependencies

You should create a python environment (e.g., with [conda](https://docs.conda.io/en/latest/)) and install the dependencies listed in the [requirements.txt](requirements.txt) file.

A short summary of commands required to have all in place is the following:

```bash
conda create -n slam-2d python=3.6
conda activate slam-2d
conda install pip
pip install -r requirements.txt
```

## Brief Notes on Simultaneous Localization and Mapping (SLAM)

The [Graph SLAM Algorithm](http://robots.stanford.edu/papers/thrun.graphslam.pdf) was introduced by Thrun et al. in 2006. It is an easy to understand method which is able to derive the robot's position estimate in world coordinates, as well as the location of key landmarks in that world, i.e., the map.

$$ \mu = \Omega^{-1}\xi $$


![Graph SLAM](./images/motion_constraint.png)

![2D Graph SLAM](./images/constraints2D.png)

![Map: Estimated robot pose and landmarks](./images/robot_world.png)

## Improvements, Next Steps

This is in reality a very simplistic example of Graph SLAM in 2D, so *many* extensions could be added. Some of them could be

- [ ] Visualize the path of the robot and how the estimated position and landmark coordinates vary.
- [ ] Update `omega` and `xi` and compute `mu` every time step; that way we could perform SLAM only tracking the last robot pose.
- [ ] Package everything into a module to make it usable with a small wheeled robot.

## Interesting Links

- [My notes and code](https://github.com/mxagar/computer_vision_udacity) on the [Udacity Computer Vision Nanodegree](https://www.udacity.com/course/computer-vision-nanodegree--nd891). A more detailed explanation of the Graph SLAM algorithm is given in the module [`04_Object_Tracking_Localization`](https://github.com/mxagar/computer_vision_udacity/blob/main/04_Object_Tracking_Localization/CVND_Object_Tracking_Localization.md).
- [A Tutorial on Graph-Based SLAM](http://ais.informatik.uni-freiburg.de/teaching/ws11/robotics2/pdfs/ls-slam-tutorial.pdf)
- [The GraphSLAM Algorithm with Applications to Large-Scale Mapping of Urban Structures, Thrun et al., 2006](http://robots.stanford.edu/papers/thrun.graphslam.pdf)
- Implementation of SLAM that uses reinforcement learning and probabilistic motion models: [Active Neural Localization (Paper)](https://arxiv.org/abs/1801.08214) | [Github Repository](https://github.com/devendrachaplot/Neural-Localization).

## Authorship

Mikel Sagardia, 2022.  
No guarantees.

You are free to use this project, but please link it back to the original source.
