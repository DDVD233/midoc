# MIB Server Documentation

## Introduction

The MIB Server is a shared computational resource with 8 NVIDIA RTX PRO 6000 GPUs (96GB memory each) available for our research group. This guide will help you get started with accessing and using the MIB server effectively.

## MIB Server GPU Utilization Dashboard

<iframe src="https://mib.media.mit.edu/d-solo/bdl3vqwxprhtsa/nvitop-dashboard?orgId=1&timezone=browser&var-hostname=$__all&var-username=$__all&refresh=5s&theme=dark&panelId=7&__feature.dashboardSceneSolo=true" width="100" height="100" frameborder="0"></iframe>

<iframe src="https://mib.media.mit.edu/d-solo/bdl3vqwxprhtsa/nvitop-dashboard?orgId=1&timezone=browser&var-hostname=$__all&var-username=$__all&refresh=5s&theme=dark&panelId=8&__feature.dashboardSceneSolo=true" width="800" height="100" frameborder="0"></iframe>

<iframe src="https://mib.media.mit.edu/d-solo/6c02270b-3b32-41c5-9ab1-4bed0f89bc3e/mib-status?orgId=1&timezone=browser&refresh=5s&theme=dark&panelId=1&__feature.dashboardSceneSolo=true" width="900" height="220" frameborder="0"></iframe>

<iframe src="https://mib.media.mit.edu/d-solo/6c02270b-3b32-41c5-9ab1-4bed0f89bc3e/mib-status?orgId=1&timezone=browser&refresh=5s&theme=dark&panelId=2&__feature.dashboardSceneSolo=true" width="900" height="220" frameborder="0"></iframe>

## Getting Started

### Prerequisites

Before you begin, ensure you have:

- Approval to use the server (see below)

- A terminal application (Terminal on macOS/Linux, or PuTTY/Windows Terminal on Windows)

- Basic familiarity with Linux command line

### Requesting Access

To get access to the MIB server:

1. **Get approval from Paul** if you are an intern or collaborator. If you are an official member of the lab, you don't need additional approval.
2. **Contact David** at dvdai@mit.edu via email or Slack to request account creation.
3. David will create your account with:
   - Username: `<your_username>` (typically your MIT username)
   - Default password: `<username>@mitmi`

!!! warning
    You will be required to change your password on first login. Choose a strong, secure password.

## SSH Access

### Basic SSH Connection

To connect to the MIB server, open your terminal and use:

```bash
ssh <username>@mib.media.mit.edu
```

For example, if your username is `dvdai`:
```bash
ssh dvdai@mib.media.mit.edu
```

!!! note
    Remember your new password! There's no automatic password recovery system, so you'll need to contact David if you forget it.

### Password-Free SSH Setup

After your first login, you can set up SSH keys to avoid typing your password every time:

```bash
# On your local machine, generate an SSH key if you don't have one
ssh-keygen -t rsa -b 4096

# Copy your key to the MIB server
ssh-copy-id <username>@mib.media.mit.edu
```

This will ask for your password one last time. After this, you can connect without entering a password.

## Environment Setup

### Installing Miniconda

You probably use Python if you are a member of our group. We recommend using Miniconda to manage your Python environments and packages:

```bash
# Create a directory for Miniconda
mkdir -p ~/miniconda3

# Download the installer
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh

# Install Miniconda
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3

# Remove the installer to save space
rm ~/miniconda3/miniconda.sh

# Activate Miniconda for current session
source ~/miniconda3/bin/activate

# Initialize conda for all shells
conda init --all
```

!!! success
    After running these commands, close and reconnect to your SSH session. You should see `(base)` before your prompt, indicating conda is active.

### Creating a Conda Environment

Create isolated environments for different projects:

```bash
# Create a new environment with specific Python version
conda create --name myproject python=3.12

# Activate the environment
conda activate myproject

# Install PyTorch with CUDA support
pip install torch torchvision

# Verify the installation
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}')"
python -c "import torch; print(f'Number of GPUs: {torch.cuda.device_count()}')"
```

## Using Persistent Sessions with tmux

Since the MIB server doesn't have a job scheduler, you'll run processes directly. Use tmux to keep them running even if your SSH connection drops:

```bash
# Start a new tmux session
tmux

# Or start with a named session
tmux new -s training

# Detach from session (keeps it running)
# Press: Ctrl+b, then d

# List active sessions
tmux ls

# Reattach to a session
tmux attach -t training

# Kill a session when done
tmux kill-session -t training
```

## GPU Usage and Management

The MIB server has 8 NVIDIA RTX PRO 6000 GPUs, each with 96GB of memory. Unlike cluster systems with SLURM, there's no automatic resource allocation - please be considerate of others.

Check current GPU usage with:
```bash
nvidia-smi
```

You'll see output similar to this:

```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.65.06              Driver Version: 580.65.06      CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|=========================================+========================+======================|
|   0  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:01:00.0 Off |                  Off |
| 30%   26C    P8              3W /  300W |   45678MiB /  97887MiB |     85%      Default |
+-----------------------------------------+------------------------+----------------------+
|   1  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:21:00.0 Off |                  Off |
| 30%   26C    P8              3W /  300W |   67890MiB /  97887MiB |     90%      Default |
+-----------------------------------------+------------------------+----------------------+
|   2  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:41:00.0 Off |                  Off |
| 30%   26C    P8             11W /  300W |   23456MiB /  97887MiB |     45%      Default |
+-----------------------------------------+------------------------+----------------------+
|   3  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:61:00.0 Off |                  Off |
| 30%   26C    P8              2W /  300W |   78901MiB /  97887MiB |     95%      Default |
+-----------------------------------------+------------------------+----------------------+
|   4  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:81:00.0 Off |                  Off |
| 30%   27C    P8              6W /  300W |   34567MiB /  97887MiB |     60%      Default |
+-----------------------------------------+------------------------+----------------------+
|   5  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:A1:00.0 Off |                  Off |
| 30%   26C    P8              3W /  300W |   12345MiB /  97887MiB |     30%      Default |
+-----------------------------------------+------------------------+----------------------+
|   6  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:C1:00.0 Off |                  Off |
| 30%   25C    P8             11W /  300W |       0MiB /  97887MiB |      0%      Default |
+-----------------------------------------+------------------------+----------------------+
|   7  NVIDIA RTX PRO 6000 Blac...    Off |   00000000:E1:00.0 Off |                  Off |
| 30%   26C    P8              4W /  300W |       0MiB /  97887MiB |      0%      Default |
+-----------------------------------------+------------------------+----------------------+
```

In this example:
- GPUs 0-5 are in use (showing memory allocation)
- GPUs 6-7 are available (0MiB memory usage)

Now you can choose any of the free GPUs (6 or 7 in this case) for your tasks. In general, follow these guidelines:

1. **Always check availability first** with `nvidia-smi`

2. **Prioritize higher-numbered GPUs** i.e. (6, 7), as others may forget to select specific GPUs and occupy lower-numbered ones (0-3). In addition, GPUs 0-3 are in the same block, while 4-7 are in another block. Whenever possible, try to use GPUs from the same block for multi-GPU training.

4. **Check for idle processes** - if you see memory allocated but 0% utilization for extended periods, consider reaching out to the user or David. 

??? tip "GPU Interconnect Topology"
    The RTX PRO 6000 GPUs are connected in a hybrid topology:
    
    |      | GPU0 | GPU1 | GPU2 | GPU3 | GPU4 | GPU5 | GPU6 | GPU7 | CPU Affinity    |
    |------|------|------|------|------|------|------|------|------|-----------------|
    | GPU0 | X    | NODE | NODE | NODE | SYS  | SYS  | SYS  | SYS  | 0-31,64-95      |
    | GPU1 | NODE | X    | NODE | NODE | SYS  | SYS  | SYS  | SYS  | 0-31,64-95      |
    | GPU2 | NODE | NODE | X    | NODE | SYS  | SYS  | SYS  | SYS  | 0-31,64-95      |
    | GPU3 | NODE | NODE | NODE | X    | SYS  | SYS  | SYS  | SYS  | 0-31,64-95      |
    | GPU4 | SYS  | SYS  | SYS  | SYS  | X    | NODE | NODE | NODE | 32-63,96-127    |
    | GPU5 | SYS  | SYS  | SYS  | SYS  | NODE | X    | NODE | NODE | 32-63,96-127    |
    | GPU6 | SYS  | SYS  | SYS  | SYS  | NODE | NODE | X    | NODE | 32-63,96-127    |
    | GPU7 | SYS  | SYS  | SYS  | SYS  | NODE | NODE | NODE | X    | 32-63,96-127    |

    GPUs with `node` connection have faster link speed than `sys` connection. For multi-GPU training, try to use GPUs within the same block (0-3 or 4-7) for better performance.

### Controlling GPU Access with CUDA_VISIBLE_DEVICES

Since there's no job scheduler, you control which GPUs your code uses with the `CUDA_VISIBLE_DEVICES` environment variable:

```bash
# Use only GPU 6
CUDA_VISIBLE_DEVICES=6 python train.py

# Use GPUs 6 and 7
CUDA_VISIBLE_DEVICES=6,7 python train.py
```

When you set `CUDA_VISIBLE_DEVICES=6,7`, your code will see these as GPU 0 and 1. PyTorch/TensorFlow will automatically map them correctly. You do NOT need to add something like `device = torch.device("cuda:6")` in your code.

For example, in PyTorch:

```python
# In your Python script, after setting CUDA_VISIBLE_DEVICES=6,7
import torch

# This will use GPU 6 (appears as device 0 to PyTorch)
device = torch.device("cuda:0")

# This will use GPU 7 (appears as device 1 to PyTorch)
device = torch.device("cuda:1")

# Check available GPUs from Python
print(f"Number of GPUs visible: {torch.cuda.device_count()}")  # Will show 2
```

### Monitoring Your GPU Usage

While your code is running, monitor GPU usage in another terminal:

```bash
# Watch GPU usage in real-time (updates every 2 seconds)
watch -n 2 nvidia-smi

# Or use nvidia-smi with loop
nvidia-smi -l 2

# Check only specific GPUs
nvidia-smi -i 6,7
```

To see who is using which GPUs:
```bash
# Show all processes using GPUs
nvidia-smi | grep python

# Or use the query format for cleaner output
nvidia-smi --query-compute-apps=pid,process_name,used_gpu_memory --format=csv
```

## Storage Management

### Storage Overview

The MIB server has the following storage that are shared among users:

| Storage Location | Path | Capacity             | Purpose  |
|-----------------|------|----------------------|---------|
| Home Directory | `/home/<username>` | 50 GB per user       | Scripts, code, small files |
| Scratch Storage | `/scratch` | 28 TB shared for all | Datasets, checkpoints, outputs |

The `/scratch` directory is also soft linked to your home directory for convenience. You can access it via `~/scratch`.

!!! note
    We only have a total of 880 GB in `/home` for all users combined. Please use the `scratch` folder whenever possible. You can create your own subdirectories in `/scratch` via `mkdir -p ~/scratch/<your_folder>`.

### Useful Commands

Monitor and manage your storage usage:

```bash
# Check your home directory usage
du -sh ~/*

# Find large files in home
find ~ -type f -size +100M -exec ls -lh {} \;

# Check scratch usage
du -sh /scratch/$USER

# Find old files in scratch (not accessed in 30 days)
find /scratch/$USER -type f -atime +30 -exec ls -lh {} \;

# Clean conda cache to save space
conda clean --all

# Clean pip cache
pip cache purge
```

Keep track of system resources:

```bash
# Check overall system resources
btop
```

## Common Issues and Solutions

We will add common issues and troubleshooting tips here as they arise. If you encounter any problems, feel free to reach out to David. If you fixed an issue yourself, consider documenting it [in our Github Repo](https://github.com/DDVD233/midoc/issues) for future users. You can simply create a new issue with the details.

### NCCL Error

The most common error you will see with the Blackwell GPUs is the NCCL error, which may show up as a generic error in PyTorch or TensorFlow. This is because many packages (i.e. vLLM) ship with NCCL that are too old for the RTX PRO 6000 GPUs. To fix this, simply uninstall NCCL from your pip:

```bash
pip uninstall nvidia-nccl-cu12
```

The program should then use the system NCCL that is compatible.

!!! success
    You're now ready to use the MIB server! Remember to be considerate of other users, communicate when you need significant resources, and clean up after your experiments. Happy computing!