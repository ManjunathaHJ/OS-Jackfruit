# 🚀 Multi-Container Runtime System (OS Jackfruit)

A lightweight container runtime built as part of the **OS Jackfruit problem**, demonstrating core Operating System concepts using low-level Linux primitives.

This project implements a simple container engine using `chroot`, process management, inter-process communication (IPC), logging, and kernel-level monitoring.

---

## 👥 Team Information

- **Manjunatha H J** — SRN: PES1UG24AM157  
- **Mohit S** — SRN: PES1UG24AM166 

---

## ⚙️ Features

- Multi-container execution (`alpha`, `beta`)
- Supervisor-based container lifecycle management
- CLI commands: `start`, `stop`, `ps`, `logs`
- IPC using UNIX domain sockets
- Logging using pipes
- CPU vs I/O workload scheduling demonstration
- Kernel module monitoring (soft & hard limits)
- Clean teardown with no zombie processes

---

## 🛠️ Setup & Execution

### Build Project
```bash
make
```

### Load Kernel Module
```bash
sudo insmod monitor.ko
```

### Start Supervisor
```bash
sudo ./engine supervisor
```

### Prepare Root Filesystems
```bash
cp -a rootfs-base rootfs-alpha
cp -a rootfs-base rootfs-beta
```

### Run Containers
```bash
sudo ./engine start alpha ../rootfs-alpha /cpu_hog 20
sudo ./engine start beta  ../rootfs-beta  /cpu_hog 20
```

### Scheduling Experiment
```bash
sudo ./engine start alpha ../rootfs-alpha /cpu_hog 20
sudo ./engine start beta  ../rootfs-beta  /io_pulse
```

### Check Status
```bash
sudo ./engine ps
```

### View Logs
```bash
sudo ./engine logs alpha
sudo ./engine logs beta
```

### Stop Containers
```bash
sudo ./engine stop alpha
sudo ./engine stop beta
```

### Kernel Monitoring
```bash
sudo ./engine start alpha ../rootfs-alpha /memory_hog
sudo dmesg | grep monitor
```

### Cleanup
```bash
sudo pkill -9 engine
sudo rm -f /tmp/engine_socket
```

---
## Demo Screenshots  
### 1.Multi-container supervision
<img src="https://github.com/user-attachments/assets/05c4e549-96ff-4781-897a-f1b8ba8956c4" width="100%">

---

### Metadata tracking
<img src="https://github.com/user-attachments/assets/d24ad86c-edff-43aa-a1b8-1aec98601033" width="100%">

---

### Logging output
<img src="https://github.com/user-attachments/assets/1a317fd9-72e5-4c96-9822-911cfe9b179c" width="100%">

---

### CLI and IPC
<img src="https://github.com/user-attachments/assets/957d5fa3-bdeb-4eb0-a045-59ce97461d1b" width="100%">

---

### Soft-limit warning
<img src="https://github.com/user-attachments/assets/a631740d-3c71-4529-8d54-307c996beb7f" width="100%">

---

### Hard-limit enforcement
<img src="https://github.com/user-attachments/assets/47e4c2a8-becd-4484-8792-4b5eb28ae986" width="100%">

---

### Scheduling experiment
<img src="https://github.com/user-attachments/assets/9ca57fd3-a5ed-4c9b-acd1-44bddfda31b0" width="100%">

---

### Clean teardown
<img src="https://github.com/user-attachments/assets/69d5d0eb-60c6-402e-9629-c95d5868a712" width="100%">

---
## 🧠 System Design

### Container Isolation
- Implemented using `chroot`
- Each container runs as a separate process
- No full namespace isolation

---

### Supervisor
- Central controller for all containers
- Handles lifecycle management
- Ensures no zombie processes

---

### IPC (CLI ↔ Supervisor)
- Implemented using UNIX domain sockets
- CLI sends commands to supervisor
- Supervisor processes and responds

---

### Logging
- Pipes capture container output
- Logs accessed using `engine logs`
- Separate logs per container

---

### Kernel Monitoring
- Implemented via `monitor.ko`
- Simulates:
  - Soft limit warnings  
  - Hard limit enforcement  

Example:
```
monitor: Registered container alpha
monitor: Soft limit exceeded for alpha
monitor: Killing container alpha
```

---

### Scheduling Behavior

- **CPU-bound (`cpu_hog`)** → continuous execution  
- **I/O-bound (`io_pulse`)** → periodic execution  

➡ Demonstrates fair scheduling

---

## ⚖️ Design Decisions

| Choice | Reason | Limitation |
|------|--------|-----------|
| chroot | Simple isolation | Not fully secure |
| Single supervisor | Easy control | Single point of failure |
| Pipes | Simple logging | Limited scalability |
| Kernel simulation | Easy demo | Not real monitoring |

---

## 📊 Observations

- Multiple containers run concurrently  
- IPC works correctly  
- Logs capture execution behavior  
- Kernel module generates expected output  
- Scheduler distributes CPU fairly  

---

## 🧾 Conclusion

This project demonstrates:

- Process management  
- Containerization  
- IPC mechanisms  
- Logging systems  
- Kernel interaction  
- Scheduling behavior  

A simple but functional container runtime built using Linux primitives.
