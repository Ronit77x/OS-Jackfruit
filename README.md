# Supervised Multi-Container Runtime with Kernel Memory Monitor

## 📌 Overview

This project implements a lightweight container runtime in user space along with a Linux kernel module for monitoring and enforcing memory limits on containers.

The system consists of:

* User-space runtime (engine.c)
* Kernel module (monitor.c)

---

## 🧱 Architecture

### User Space (engine.c)

* Supervisor process using UNIX domain sockets
* Container lifecycle management (run, stop, ps, logs)
* Logging system:

  * Bounded buffer (producer-consumer)
  * Logging thread
  * File-based logs (`logs/<container>.log`)
* Graceful shutdown (SIGTERM → SIGKILL)
* Kernel interaction via ioctl

---

### Kernel Space (monitor.c)

* Character device: `/dev/container_monitor`
* Linked list of monitored containers
* Periodic monitoring using kernel timer
* Soft limit → warning (dmesg)
* Hard limit → process killed
* Mutex for synchronization

---

## ⚙️ Features

* Multi-container support
* Memory monitoring and enforcement
* Logging system (thread + buffer)
* IPC using UNIX domain sockets
* Graceful shutdown
* Kernel-user communication

---

## 🚀 How to Run

### Build

make

### Load Kernel Module

sudo insmod monitor.ko

### Start Supervisor

sudo ./engine supervisor rootfs-base

### Run Container

sudo ./engine run alpha ./rootfs-alpha ./cpu_hog

### Stop Container

sudo ./engine stop alpha

### View Logs

sudo ./engine logs alpha

---

## 📸 Expected Output

* Container start/stop messages
* Logs printed in terminal
* Kernel logs via `dmesg`
* Log files in `logs/`

---

## 📂 Structure

* engine.c
* monitor.c
* monitor_ioctl.h
* Makefile
* logs/

---

## ✅ Conclusion

This project demonstrates:

* Process management
* Kernel programming
* Synchronization
* IPC and system design
