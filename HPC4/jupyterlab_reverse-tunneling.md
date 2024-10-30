# Running JupyterLab on Slurm Compute Nodes via Reverse Tunneling

This tutorial demonstrates how to run JupyterLab (or Jupyter Notebook) on a Slurm compute node and access it from your local machine.

## Prerequisites
- Access to a Slurm cluster
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
jupyter-lab --no-browser --ip=0.0.0.0 --port=8888 &
```
Note: Save the token or URL displayed with 127.0.0.0 in the output.

### 3. Create Reverse Tunnel to Login Node
Forward the compute node's JupyterLab port to the login node:
```bash
ssh -N -R 18888:localhost:8888 login1 &
```
Note that for HPC4, there are two login nodes (login1 and login2) for load balancing purpose. Double check which login node you are logged into in the next step.

### 4. Create Local Tunnel
On your local machine, create a tunnel to the login node:
```bash
ssh -N -L 8888:localhost:18888 username@hpc4.ust.hk
```

### 5. Access JupyterLab
1. Open your web browser
2. Navigate to `http://127.0.0.1:8888` or the URL with token
3. Enter the token from step 2 if prompted

## Tips
- Choose unique port numbers to avoid conflicts with other users
- Use `ps aux | grep ssh` to check tunnel status
- Use `kill %N` or `fg` to manage background processes (%N means %1, %2 etc.. Use `jobs` to check the processes id)

## Cleanup
When finished:
1. Close browser
2. Kill SSH tunnels: `kill %N` or `pkill ssh`
3. Exit Slurm session
