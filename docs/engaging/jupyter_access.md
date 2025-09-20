## Running Jupyter Notebooks

Jupyter notebooks are great for interactive development. Before following these steps, ensure you have a working Miniconda/Python environment following the [Getting Started Guide](/engaging/getting_started).

First, install JupyterLab in your environment:

```bash
# Activate your environment
conda activate myproject

# Install JupyterLab and useful extensions
pip install jupyterlab ipywidgets matplotlib

# Install kernel for this environment
python -m ipykernel install --user --name myproject --display-name "Python (myproject)"
```

#### Step 1: Request a GPU node with resources

```bash
srun --gres=gpu:1 --time=12:00:00 -p pi_ppliang --nodelist=node2500 \
     -c4 --mem=32G --pty bash
```

Adjust the resources based on your needs. Jupyter itself doesn't need much, but your computations might.

#### Step 2: Activate your environment

```bash
conda activate myproject
```

#### Step 3: Start JupyterLab on the compute node

```bash
# Choose a port number between 1024-65535 (e.g., 8888, 9999, 1717)
jupyter-lab --ip=0.0.0.0 --port=1717 --no-browser
```

The output will show something like:
```
[I 2024-01-15 10:30:00.123 ServerApp] Jupyter Server is running at:
[I 2024-01-15 10:30:00.123 ServerApp] http://node2500:1717/lab?token=abc123def456...
[I 2024-01-15 10:30:00.123 ServerApp] http://127.0.0.1:1717/lab?token=abc123def456...
```

Keep this terminal open! Copy the token from the URL - you'll need it.

#### Step 4: Create SSH tunnel (on your local machine)

Open a **new terminal on your local computer** and run:

```bash
ssh -N -L 1717:node2500:1717 <mit_username>@orcd-login001.mit.edu
```

This creates a tunnel:
- `-N`: No command execution, just forwarding
- `-L 1717:node2500:1717`: Forward local port 1717 to node2500 port 1717

!!! note
    This terminal will appear to hang - that's normal! It's maintaining the tunnel.

#### Step 5: Access Jupyter in your browser

Open your web browser and navigate to:
```
http://127.0.0.1:1717/lab?token=abc123def456...
```

Use the complete URL with token from Step 3.

#### Step 6: Clean up when done

Always clean up to free resources:

1. Save your work in Jupyter

2. In Jupyter: File → Shut Down

3. On compute node terminal: Press `Ctrl+C` to stop JupyterLab

4. On local terminal: Press `Ctrl+C` to stop the SSH tunnel

5. Type `exit` on compute node to release the resources