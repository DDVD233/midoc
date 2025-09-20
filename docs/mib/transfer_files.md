The MIB server has the following storage that are shared among users:

| Storage Location | Path | Capacity             | Purpose  |
|-----------------|------|----------------------|---------|
| Home Directory | `/home/<username>` | 50 GB per user       | Scripts, code, small files |
| Scratch Storage | `/scratch` | 28 TB shared for all | Datasets, checkpoints, outputs |

The `/scratch` directory is also soft linked to your home directory for convenience. You can access it via `~/scratch`.
If you want to transfer files to/from this folder, you will likely need to first transfer to/from your personal storage (pool at `/home/<username>/orcd/pool`, or scratch at `/home/<username>/orcd/scratch`), then move files while on the compute node.

#### Uploading Files to the Cluster

The easiest way to transfer files is using `rsync` over SSH. From your local machine, run:
```bash
# Basic syntax
rsync -avz <local_path> <mit_username>@mib.media.mit.edu:<remote_path>

# Upload a single file
rsync -avz ~/Documents/data.csv dvdai@mib.media.mit.edu:~/

# Upload an entire directory
rsync -avz ~/Documents/project/ dvdai@mib.media.mit.edu:~/project/

# Upload with progress bar
rsync -avz --progress ~/largefile.zip dvdai@mib.media.mit.edu:~/
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
rsync -avz dvdai@mib.media.mit.edu:~/results.csv ./

# Download a directory
rsync -avz dvdai@mib.media.mit.edu:/scratch/project/ ~/Downloads/project/
```

#### Downloading from the Internet

On the cluster, you can also download files directly from the internet using `wget` or `curl`:
```bash
# Direct download with wget
wget https://example.com/dataset.zip

# Download with custom filename
wget -O mydata.zip https://example.com/dataset.zip

# Download to specific directory
cd /scratch/datasets
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

Back to the [Getting Started Guide](/mib/getting_started).