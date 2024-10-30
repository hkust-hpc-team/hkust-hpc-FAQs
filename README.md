# hkust-hpc-FAQs
FAQs for HKUST hpcs (SuperPOD, HPC4)

# General usage
Q: Why I got "QOSMinGRES" error?
A: When allocating GPU nodes, please use --gres, --gpus-per-node or --gpus-per-task to specify the number of GPU requested.

# Commands
Run a overlap job:
```bash
srun -A <account> --overlap --jobid <jobid> --pty bash
```
