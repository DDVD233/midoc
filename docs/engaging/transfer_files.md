The cluster provides different storage areas for different purposes:

| Storage Type | Path | Quota | Backed up | Purpose/Notes |
|--------------|------|-------|-----------|---------------|
| Home Directory Flash | `/home/<username>` | 200 GB | Backed up with snapshots | Use for important files and software |
| Pool Hard Disk | `/home/<username>/orcd/pool` | 1 TB | Disaster recovery backup | Storing larger datasets |
| Scratch Flash | `/home/<username>/orcd/scratch` | 1 TB | Not backed up | Scratch space for I/O heavy jobs |

On our node, we also have a total of 28TB NVMe SSD mounted at `/scratch`, which is not backed up. You can use this for high-speed temporary storage. This folder is not accessible from the login node. 

If you want to transfer files to/from this folder, you will likely need to first transfer to/from your personal storage (pool at `/home/<username>/orcd/pool`, or scratch at `/home/<username>/orcd/scratch`), then move files while on the compute node.

#### Uploading Files to the Cluster

The easiest way to transfer files is using `rsync` over SSH. From your local machine, run:
```bash
# Basic syntax
rsync -avz <local_path> <mit_username>@orcd-login001.mit.edu:<remote_path>

# Upload a single file
rsync -avz ~/Documents/data.csv dvdai@eofe7.mit.edu:~/

# Upload an entire directory
rsync -avz ~/Documents/project/ dvdai@eofe7.mit.edu:~/project/

# Upload with progress bar
rsync -avz --progress ~/largefile.zip jdoe@eofe7.mit.edu:/pool001/jdoe/
```

Useful `rsync` flags:

- `-a`: Archive mode (preserves permissions, timestamps)

- `-v`: Verbose (shows files being transferred)

- `-z`: Compression (faster for text files)

- `--progress`: Shows transfer progress

#### Downloading Files from the Cluster

From your local machine, use `rsync` to download files:

```bash
# Download a file to current local directory
rsync -avz jdoe@eofe9.mit.edu:~/results.csv ./

# Download a directory
rsync -avz jdoe@eofe9.mit.edu:~/project/ ~/Downloads/project/
```

#### Downloading from the Internet

On the cluster, you can also download files directly from the internet using `wget` or `curl`:
```bash
# Direct download with wget
wget https://example.com/dataset.zip

# Download with custom filename
wget -O mydata.zip https://example.com/dataset.zip

# Download to specific directory
cd /pool001/$USER/datasets
wget https://example.com/largefile.tar.gz
```

For Google Drive files, you can use `gdown`:
```bash
pip install gdown
gdown https://drive.google.com/uc?id=FILE_ID
```
    
For Kaggle datasets, use the Kaggle CLI:
```bash
pip install kaggle
kaggle datasets download -d dataset-name
```

Back to the [Getting Started Guide](/engaging/getting_started).