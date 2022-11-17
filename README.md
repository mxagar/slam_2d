# Landmark Detection & Robot Tracking (SLAM)

This repository contains a toy implementation of 2D SLAM system. A map of the environment is created only from sensor and motion gathered from a simulated robot moving over time and space. The location of the robot is tracked as well as landmarks in the scene.

Th starter code was taken from a project from the [Udacity Computer Vision Nanodegree](https://www.udacity.com/course/computer-vision-nanodegree--nd891), available in its original form in the repository [udacity/P3_Implement_SLAM](https://github.com/udacity/P3_Implement_SLAM).

## Introduction

:construction:

*Preliminary Results*

Table of Contents:

- [Landmark Detection & Robot Tracking (SLAM)](#landmark-detection--robot-tracking-slam)
  - [Introduction](#introduction)
  - [How to Use This](#how-to-use-this)
    - [Overview of Files and Contents](#overview-of-files-and-contents)
    - [Dependencies](#dependencies)
  - [Brief Notes on Simultaneous Localization and Mapping (SLAM)](#brief-notes-on-simultaneous-localization-and-mapping-slam)
  - [Practical Notes](#practical-notes)
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

- [`1. Robot Moving and Sensing.ipynb`](1. Robot Moving and Sensing.ipynb)
- [`2. Omega and Xi, Constraints.ipynb`](2. Omega and Xi, Constraints.ipynb)
- [`3. Landmark Detection and Tracking.ipynb`](3. Landmark Detection and Tracking.ipynb)

On the other hand, the implementation scripts and their contents are the following:

- [`robot_class.py`](robot_class.py)
- [`helpers.py`](helpers.py)

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

:construction:

## Practical Notes

:construction:

## Improvements, Next Steps

- [ ] A
- [ ] B

## Interesting Links

- [My notes and code](https://github.com/mxagar/computer_vision_udacity) on the [Udacity Computer Vision Nanodegree](https://www.udacity.com/course/computer-vision-nanodegree--nd891).
- Implementation of SLAM that uses reinforcement learning and probabilistic motion models: [Active Neural Localization (Paper)](https://arxiv.org/abs/1801.08214) | [Github Repository](https://github.com/devendrachaplot/Neural-Localization).

## Authorship

Mikel Sagardia, 2022.  
No guarantees.

You are free to use this project, but please link it back to the original source.
