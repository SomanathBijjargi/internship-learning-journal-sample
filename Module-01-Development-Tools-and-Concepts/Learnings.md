# Week 1, Session 1 

## Learnings of Session 1 :

**1. What is WSL? Why it is Used?**

WSL = Windows Subsystem for Linux

It’s a feature of Microsoft Windows that lets you run a real Linux environment directly on Windows — no dual-boot, no heavy virtual machine.

You can install Linux distros like:
```
1.Ubuntu
2.Debian
3.Kali Linux
4.Fedora
```
| Feature            | **WSL**                | **Virtual Machine**  |
| ------------------ | ---------------------- | -------------------- |
| **Host OS**            | Microsoft Windows only | Any OS               |
| **Linux Kernel**      | Real kernel (WSL2)     | Real kernel          |
| **Runs OS separately** | ❌                     | ✅                  |
| **Startup time**       | Very fast ⚡           | Slow 🐢             |
| **RAM usage**          | Low                    | High                 |
| **Disk usage**         | Low                    | High                 |
| **GUI support**        | Limited (needs setup)  | Full GUI             |
| **File sharing**       | Seamless               | Manual setup         |
| **Best for**           | Developers             | OS learning, testing |
| **Restart needed**     | ❌                      | ❌                    |

## Architecture Difference
### WSL
```
Windows
 └── Linux (WSL)
      └── Uses Windows resources directly
```
### Virtual Machine
```
Windows
 └── VirtualBox / VMware
      └── Linux OS (full system)
```