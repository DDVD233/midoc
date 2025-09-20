# Welcome to Multisensory Intelligence Computing Resources Documentation!

First of all, welcome to the Multisensory Intelligence (MI) group! We are excited to have you here and look forward to collaborating with you on various projects.

This documentation is designed to provide you with all the necessary information and resources to get started with our projects. 

## Available Resources

As of now, we have two main GPU servers available for use:

1. [**Engaging**](/engaging/getting_started): This is a server node hosted on the MIT Engaging cluster. It has 8 NVIDIA H200 GPUs, each with 144GB of memory. 

2. [**MIB** (**M**ultisensory **I**ntelligence **B**lackwell Server)](/mib/getting_started): This is a new server node hosted on the Media Lab cluster. It has 8 NVIDIA RTX 6000 Blackwell MaxQ GPUs, each with 96GB of memory.

In addition, MIT also offers complimentary GPU access, where you can directly request via slurm. 

## Choosing the Right Server

Generally, we recommend choosing the server that is less busy at the time you need it. However, you may want to consider the following factors:

1. **Access**: Engaging is configured with Slurm, while MIB allows direct SSH access. If your workflow uses VSCode or other IDEs that require direct access, MIB might be the better choice.
2. **GPU Memory**: If your tasks require more GPU memory (>96GB), then Engaging is the better option.
3. **Framework Compatibility**: MIB has the latest Blackwell GPUs, which requires a minimal CUDA version of 12.8. If your code relies on older Pytorch, Engaging might be more compatible.

## Getting Started

After you choose a server, please refer to the following documentation for instructions on how to set up your environment and start using the resources:

- [Getting Started with Engaging](engaging/getting_started.md)
- [Getting Started with MIB](mib/getting_started.md)