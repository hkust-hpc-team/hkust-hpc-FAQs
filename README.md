# hkust-hpc-FAQs
FAQs for HKUST hpcs (SuperPOD, HPC4)

# General usage
**Q: Why I got "QOSMinGRES" error?**

**A:** When allocating GPU nodes, please use --gres, --gpus-per-node or --gpus-per-task to specify the number of GPU requested.

**Q: Why does my SSH session frequently disconnect during long operations?**

**A:** SSH sessions can timeout due to network settings or inactivity. Here are solutions to maintain your connection:

1. Configure SSH Keep-Alive Settings:
Add these settings to your `~/.ssh/config` file:
```bash
Host ust-hpc4-login
    HostName hpc4.ust.hk
    User YOUR_USERNAME
    ServerAliveInterval 30
    ServerAliveCountMax 3
    TCPKeepAlive yes
```

2. Use Terminal Multiplexers, which keep the sessions alive when disconnect:
   - GNU Screen:
     ```bash
     # Start new session
     screen
     # Reconnect
     screen -r
     ```
   - Tmux:
     ```bash
     # Start new session
     tmux
     # Reconnect
     tmux attach
     ```

3. For Long Operations:
   - Use batch jobs instead of interactive sessions
   - Always run important processes in Screen/Tmux
   - Consider using `nohup` for background processes

# Commands
For running an overlap job:
```bash
srun -A <account> --overlap --jobid <jobid> --pty bash
```
