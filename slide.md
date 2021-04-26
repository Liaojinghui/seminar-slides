---
title: "Arm ETM/PMU"
author: "Haonan Li, Wenxuan Shi, Xueying Zhang"
institute: "COMPASS"
urlcolor: blue
colortheme: "beaver"
date: "April 20, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
header-includes: |
  \usepackage[normalem]{ulem}
---

# Haonan Li

## Plan of Last Week

- Syscall capturer: handle more and more syscalls...
- Paper writing: revise & rewrite
- Make pressure experiments for my syscall capturer

---

## Progress

- Syscall capturer: add support for `poll/epoll`
  - Finish _all_ (almost) syscalls used in nginx: `recvfrom`, `openat`, `sendfile`, `epoll`
- Paper writing: revise & rewrite
  - rewrite subsection: _Desgin-Challenge_ and desgin for my syscall capturer
  - _catch up with Zhenyu’s proofreading_
- Make fomal experiments
  - delayed to today & tomorrow
  - setting up environment... as we found we should run in SSD
  - Wenxuan: _why not directly mount and write to SSD_
  - environment: finished by last night

<!-- ---

## Epoll

- `int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout)`
- `events` represents `array` of `struct epoll_event`
- The length of array (the part further used by user) is `return value`
- **Variable-length array!** -->

---

## Syscall Types

- **Scheduling related**: e.g., `wait` and `nanosleep`, we simply ignore
- **Change State of OS**: e.g., `epoll_create` and `chroot`, ignore again as it will not affect the user's program
- **Get form OS**: e.g., `getpid`, `getcpu` and `getcwd`, record its return value and memory region change
- **Input**: e.g., `read` and `recvfrom`, truncate their contents
- **Output**: ignore again

---

## Plan for Next Week

- Do our best to make zhenyu spend a happy holiday

# Xueying

---

## Xueying

- Last week's plan: write paper
- This week's work: write paper (10%), do my homework and project (90%)
- Nest week's plan: revise my part, intensively. 

# Wenxuan

---

## Last week's plan

- [x] Writing the non-concurrency evaluation part

---

## Revise Progress

| Section Name   | # of zhenyu's advice | # of fixed |
| -------------- | -------------------- | ---------- |
| Abstract       | -                    | -          |
| Introduction   | 92                   | 0          |
| Background     | 47                   | 13         |
| Design         | 205                  | 43         |
| Implementation | -                    | -          |
| Evaluation     | -                    | -          |
| Discussion     | -                    | -          |
| Related Work   | -                    | -          |

---

## Background: Revise Progress

|                      | # of advice | # of fixed | Writer  |
| -------------------- | ----------- | ---------- | ------- |
| Concurrency Bug      | 28          | 0          | Xueying |
| ETM                  | 13          | 13         | Haonan  |
| Hardware Breakpoints | 14          | 0          | YiMing  |

---

## Design: Revise Progress

|                       | # of advice | # of fixed | Writer |
| --------------------- | ----------- | ---------- | ------ |
| **Overview**          | 8           | 6          | Haonan |
| **Design Challenges** | 39          | 21         | Haonan |
| **Online Record**     |             |            |        |
| ETM Manager           | 9           | 0          | Haonan |
| Syscall Capturer      | 18          | 16         | Haonan |
| **Offline Analysis**  |             |            |        |
| Control Flow Builder  | 24          | 0          | YuXin  |
| Root Cause Detector   | 101         | 0          | YiMing |

---

## Next week's plan

| Section Name   | # of zhenyu's advice | # of fixed |
| -------------- | -------------------- | ---------- |
| Abstract       | -                    | -          |
| Introduction   | 92                   | 0          |
| Background     | 47                   | 13         |
| Design         | 205                  | 43         |
| Implementation | -                    | -          |
| Evaluation     | -                    | -          |
| Discussion     | -                    | -          |
| Related Work   | -                    | -          |

Huge workload! Help finish the revise process.