# Python Guide for Bunya HPC

## Introduction
The purpose of this guide is to explain how Python is run on the Bunya HPC environment.
It provides information for people already familiar using Python on personal workstations or laptops,
who wish to use the resources provided by HPC systems such as:

- High-end accelerators (GPUs)
- High CPU parallel code  
- Large memory capacity
- High performance and capacity storage system

Many of the concepts covered here are detailed in Bunya's general user documentation. This guide focuses specifically on how these concepts relate to running Python on the Bunya HPC.

> **Note:** This guide assumes the reader has some knowledge of running Python on their own workstation or laptop but is unfamiliar with Bunya or HPC systems in general.

## Operation Modes
There are two methods of running Python programs on Bunya:
the command line interface; and the Jupyter Lab or Notebook interface.
It is not uncommon to use both methods depending on the situation.

### Command Line Interface
The command line interface is the traditional way of interacting with HPC systems.
This is done with a terminal emulator running on your local machine.
Putty (Windows), Terminal (Mac), or VS Code.

### The Jupyter Interface
Jupyter Labs or Notebook is a simple interactive interface available via Bunya's [On Demand](OnDemand-Guide.md) guide.


Several ways to interact with Bunya. These are the methods important to Python users:

| Mode        | Use Case                                                                                 |
|-------------|------------------------------------------------------------------------------------------|
| Interactive | Create environments and debug Python programs. Work should not last longer than an hour. |
| Batch       | Schedule Python programs to run on the queuing system.                                   |
| On Demand   | Work interactively in a friendly environment using Jupyter Labs or Notebooks.            |


## The Module System
Bunya uses a module system to make software components (such as Python) available to users.