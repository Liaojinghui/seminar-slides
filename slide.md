---
title: "Arm ETM/PMU"
author: "Yiming Zhang, Yuxin Hu, Haonan Li, Wenxuan Shi, Xueying Zhang"
institute: "COMPASS"
urlcolor: blue
colortheme: "beaver"
date: "May 18, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---

# Haonan

## Plan of Last Week

- Write some random words for my graduation project. (deadline: May 14)
- Determine what the next of our project, make more improvement?
- Prepare for apper sharing: Mozilla's rr


---

## Progress

- Finished graduation project before May 14, 11:58 PM
- Discussed about the future work last Friday.
- Finding many papers 


---

## Syscord: a bug

- cannot write file after 2.1 G (found in Apr 24, 3 weeks ago)
- bypass approach: open 10 files at beginning, when a file up to 2.1 G, write to another one
- reason: an ancient problem, associated with 32-bit systems...
- `-D_FILE_OFFSET_BITS=64` or ... `O_RDWR | O_CREAT | O_LARGEFILE`

---

## Discussion for future work

- Improve syscord: 
  - More syscalls, optimization on performance and record size
  - More Non-deterministic events: interrupt, trap, signal
- Library Hook: find a general way to handle library functions


---

## Dilemma: How to handle library functions

- Ignore it: configure ETM to trace all instrctions in user space
  - result in a huge size of trace result (considering with or without `-static`)
- Record library functions, as syscalls
  - There are only 276 syscalls, while infinite numbers of library functions.
  - How to record the side effect of library functions?

---

## Try to address these problems: paper reading

\tiny
| Name     | Titile                                                       | Author Affiliation  | Conference            | Heighlight                                    |
| -------- | ------------------------------------------------------------ | ------------------- | --------------------- | --------------------------------------------- |
| RR       | **Engineering** Record And Replay For **Deployability**      | Mozilla             | ATC'17         | RnR system                                    |
| R2       | An Application-Level Kernel for Record and Replay            | Microsoft           | OSDI'08               | Library                                       |
| Castor   | Towards Practical **Default-On** Multi-Core Record/Replay    | Stanford            | ASPLOS'17             | Deafult On                                    |
| `liblog` | Replay Debugging for Distributed Applications                | Berkeley        | ATC'06         | Lib Log                                       |
| Scribe   | Transparent, Lightweight Application Execution Replay on Commodity Multiprocessor Operating Systems | Columbia  | SIGMETRICS’10 (CCF-B) | 2.5% overhead? Re-execution instrad of replay |


---

## Paper Connection of R2

Powered by Connected Papers
![](r2_connect.png){width=90%}

---

## Some other things...

- Linux AIO (asynchronous IO)
- `io_submit`, `io_getevents`
- As far as we know, RR does not support AIO: `io_setup error: Function not implemented`
- looks like the only way to handle asynchronous events is, add a delay

---
## Plan for Next Week

- Read paper, read paper, read paper, desgin for library hook
- Prepare for apper sharing: Mozilla's rr (**still NOT for next week**)

# Wenxuan

---

## Last week's plan

Replay machine

---

## This week's work

Use `process_vm_writev()` to change the data of the execution segment of the target process at runtime, directly.

The parameter of `process_vm_writev()`: pid, target address 

- step1: start target process, register interrupt handler and wait on signal
- step2: start another process, call `process_vm_writev()`, send a signal

# Xueying

---

## Last week's plan

Replay machine

---

## This week's work

1. add the ability to read from a file
2. understanding syscord and study syscall

---

## Next week's plan 

Replay machine & study syscall

