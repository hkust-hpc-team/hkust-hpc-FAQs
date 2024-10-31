# Running JupyterLab on Slurm Compute Nodes

This tutorial demonstrates how to run JupyterLab (or Jupyter Notebook) on a Slurm compute node and access it from your local machine.

## Prerequisites
- Access to the cluster
- Conda/Python environment with JupyterLab installed
- SSH access to the cluster

## Steps

### 1. Allocate a Compute Node
Request an interactive session on a compute node:
```bash
srun --account-<your account> --partition=<partition to use> --nodes=1 --ntasks-per-node=1 --cpus-per-task=16 --time=01:00:00 --pty bash
```
Note that on HPC4, maximum walltime for interactivate job is 4 hours.
### 2. Launch JupyterLab
Activate your environment and start JupyterLab:
```bash
conda activate myenv
jupyter-lab --no-browser --ip=0.0.0.0 --port=8888
```
Note: Save the token or URL displayed with 127.0.0.0 in the output.

### 3. Create Local Tunnel
On your local machine, create a ssh tunnel to the login node:
```bash
ssh -N -L 8888:<allocated node name>:18888 username@hpc4.ust.hk
```

### 4. Access JupyterLab
1. Open your web browser
2. Navigate to `http://127.0.0.1:8888` or the URL with token
3. Enter the token from step 2 if prompted

## Tips
- Choose unique port numbers to avoid conflicts with other users

## Cleanup
When finished:
1. Close browser
2. Exit Slurm session
