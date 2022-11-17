# Landmark Detection & Robot Tracking (SLAM)

This repository contains a toy implementation of Simultaneous Localization and Mapping (SLAM) in 2D which uses the [Graph SLAM Algorithm](http://robots.stanford.edu/papers/thrun.graphslam.pdf). A map of the environment is created only from sensor and motion data obtained from a simulated robot moving over time and space. The location of the robot is tracked, as well as landmarks in the scene.

Th starter code was taken from a project from the [Udacity Computer Vision Nanodegree](https://www.udacity.com/course/computer-vision-nanodegree--nd891), available in its original (incomplete) form in the repository [udacity/P3_Implement_SLAM](https://github.com/udacity/P3_Implement_SLAM).

Table of Contents:

- [Landmark Detection & Robot Tracking (SLAM)](#landmark-detection--robot-tracking-slam)
  - [How to Use This](#how-to-use-this)
    - [Overview of Files and Contents](#overview-of-files-and-contents)
    - [Dependencies](#dependencies)
  - [Brief Notes on Graph SLAM](#brief-notes-on-graph-slam)
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
├── LICENSE                                     # Original Udacity license
├── README.md                                   # This file
├── helpers.py                                  # Visualization and setup
├── images/                                     # Figures used in the notebooks
├── requirements.txt                            # Dependencies
└── robot_class.py                              # Robot with move() and sense() functions
```

The implementation is guided by the notebooks, which either contain the necessary code or import it from different scripts (explained below).

As mentioned, first, the [dependencies](#dependencies) need to be installed. Assuming everything is set up, we can run the notebooks sequentially; they carry out the following tasks:

- `1. Robot Moving and Sensing.ipynb`: the `robot` class defined in `robot_class.py` is explained, as well as some related utilities, such as map visualization and motion handling.
- `2. Omega and Xi, Constraints.ipynb`: the key aspects of the Graph SLAM are explained for the 1D case.
- `3. Landmark Detection and Tracking.ipynb`: **that's where SLAM is implemented**.

On the other hand, the implementation scripts and their contents are the following:

- [`robot_class.py`](robot_class.py): `robot` class which contains the location of the robot in a 2D world; additionally, `move()` and `sense()` functions that enable interacting with the world are provided. The *landmarks* measured in the world are created here.
- [`helpers.py`](helpers.py): auxiliary functions for visualization and world setup.

### Dependencies

You should create a python environment (e.g., with [conda](https://docs.conda.io/en/latest/)) and install the dependencies listed in the [`requirements.txt`](requirements.txt) file.

A short summary of commands required to have all in place is the following:

```bash
conda create -n slam-2d python=3.6
conda activate slam-2d
conda install pip
pip install -r requirements.txt
```

## Brief Notes on Graph SLAM

The [Graph SLAM Algorithm](http://robots.stanford.edu/papers/thrun.graphslam.pdf) was introduced by Thrun et al. in 2006. It is an easy to understand method which is able to derive the robot's position estimate in world coordinates, as well as the location of key landmarks in that world, i.e., the map.

The method consists in a cycle of two functions:

- `measure()`, which yields the distance from the robot position to reachable landmarks, accounting for a measurement error,
- and `move()`, which updates the robot position given a relative movement vector, accounting for motion error.

Thus, as the robot moves and measures, we get a sequence of relative distance measurements between consecutive robot positions and robot positions and landmarks; these sequence is effectively a set of **constraints**.

Each motion of measurement constraint is a Gaussian; for instance, in a 1D world in which we go from `x0` to `x1` with a step size of `dx` and a motion error of `sigma`, the constraint equation is given by:

$$ \frac{1}{\sqrt{2\pi\sigma}} \mathrm{exp}(-\frac{1}{2}\frac{(x_1-x_0-dx)^2}{\sigma^2}).$$

The relevant part of that constraint concentrates in the exponent, which can be reduced to be

$$ \frac{x_1}{\sigma} - \frac{x_0}{\sigma} = \frac{dx}{\sigma}.$$

The key idea behind the Graph SLAM method consists in aggregating in a linear system those reduced constraint expressions. To that end, two objects are defined:

- the `Omega` square matrix, which contains all the location and landmark variable coefficients  
- and the `xi` vector are defined, which contains the distances between two consecutive locations or a location and a landmark.

The following figure shows how `Omega` (in blue) and `xi` (in pink) are assembled. Let's say we have a 1D world with 2 landmarks `L0, L1` and three robot positions `x0, x1, x2`. Then, if we go from `x0` to `x1` with a `dx = 5`, assuming `sigma = 1`, the matrices are filled as follows:

<p align="center">
  <img src="./images/motion_constraint.png" alt="Graph SLAM" width=500px>
</p>

Note that *one constraint with two variables has two representations*, which need to be accounted for in `Omega` and `xi`. If we take another step, the constraint equations are formed analogously, and the matrix cells are updated **by adding the new constraint coefficients**. That's so because, in reality, we are multiplying Gaussians, which effectively results in adding exponents!

If we measure the distance from location/position `x1` to a landmark `L1`, the reduced constraint equation has the following form:

$$ \frac{L_1}{\sigma} - \frac{x_1}{\sigma} = \frac{dL}{\sigma}.$$

This measurement constraint is equivalent to the former motion constraint, and it is entered to the constraint matrices `Omega` and `xi` in the same fashion; only now the uncertainty `sigma` refers to the measurement error, not the movement error.

**Now the best part**: It turns out that the vector `mu` which contains the optimal solution which describes the robot path, its final location and the location of the landmarks is the following:

$$ \mu = (x_0, x_1, \dots, x_n, L_0, L_1, \dots, L_m)^{T} = \Omega^{-1}\xi.$$

As simple as that! We aggregate the constraint coefficients and distances along the time in the matrices, we invert `omega` and a simple product gives the location and the map :sunglasses:.

In order to extend the method to a 2D world, we can either:

- for each dimension, work with an independent pair `Omega` and `xi`,
- or pack the `x` and `y` coordinates of each variable one after the other, as shown in the following figure: 

<p align="center">
  <img src="./images/constraints2D.png" alt="2D Graph SLAM" width=500px>
</p>

In this mini project, the latter method is used, because it generalizes nicely the 1D case. However, note that this method creates one large and sparse `Omega` matrix which requires twice as much memory as the former method.

The final implementation takes a random set of landmarks in a 2D grid world and a noisy robot path with measurements and visualizes the estimated final robot pose (red `O`), as well as the estimated landmarks (purple `X` symbols).

<p align="center">
  <img src="./images/robot_world.png" alt="Map: Estimated robot pose and landmarks" width=500px>
</p>

If you'd like more details about the Graph SLAM algorithm, I've added some references to the section [Interesting Links](#Interesting-Links).

## Improvements, Next Steps

This is in reality a very simplistic example of Graph SLAM in 2D, so *many* extensions could be added. Some of them could be

- [ ] Visualize the path of the robot and how the estimated position and landmark coordinates vary.
- [ ] Update `Omega` and `xi` and compute `mu` every time step; that way we could perform SLAM only tracking the last robot pose.
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
