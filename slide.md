---
title: "Arm ETM/PMU"
author: "Yiming Zhang, Yuxin Hu, Haonan Li, Wenxuan Shi, Xueying Zhang"
institute: "COMPASS"
urlcolor: blue
colortheme: "beaver"
date: "May 11, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---

# Haonan

## Plan of Last Week

-  Do our best to make Zhenyu spend a happy holiday

---

## Progress

- Completely missed, Zhenyu had an **unforgettable** holiday (20 days continuous work, from Apr. 19 to May 8) 

---

## Syscall Capturer: Syscord

- key problem: \textsc{Syscord} introduces 10% overhead 
  - 2~3% on x86, juno with debian, and raspberry pi.
  - 10% on juno with open embedded (ETM available ONLY on OE)
- finding the bottomneck by comment each part of code (May. 5)
  - we found the bottomneck is `sprintf`/`strncpy`, which introduces 7%  overhead...
  - replace `strncpy` with `memcpy`, and implement `sprintf` as our own version (*Reinventing the wheel*), reduce the overhead to 6%

### Communication with Lei Zhou
Lei: You must have called some Linux API somewhere

I: No, that's impossible, I barely call any API

*(half a day latter...)*

I: Oh... I call `sprintf`... it looks like the reason

---

## On Juno: The large scale of test, the lower overhead

| Size    | baseline | syscord       |
| ------- | -------- | ------------- |
| 5,000   | 0.68     | 0.85 (+24.3%) |
| 50,000  | 7.642    | 8.40 (+12%)   |
| 500,000 | 88.94    | 90.09 (+1.3%) |
Table: test on different number of requests for nginx

But we don't find the same trend on other platforms...

---

## Paper writing: an overloaded week (or 3 days)...

![](images/commit_stat.pdf)

\color{orange} Average: 10 commits a day. \color{blue} Last three hours: 14, 10, 22 commits

---

::: columns

:::: {.column width="30%"}

![](images/metainfo.png){height=70%}

::::

:::: {.column width="60%"}

\vspace{50pt}
- generate the final version at 7:58 PM
- impossible to pass it again...
- many mistakes on it, such as writing *processor* as *professor*...
- maybe for next time, we need set up an earlier deadline for final draft.
::::

:::

---

## Other feelings...

- Communication, exchanges, synchronization
  - I have been focusing on my part until the last week.
  - We should have more meetings, especially for remote collaboration
- Very very very thanks to Zhenyu:
  - He definitely contributed the most

### Communication with Zhenyu
I: maybe you need to directly write some part, instead of giving suggestions.

Zhenyu: No, never. This is your paper.

*(time flies to May 6 & 7)*

Zhenyu: I have changed your motivation example, what do you think about it?

---

## Plan for Next Week

- Write some random words for my graduation project. (deadline: May 14)'
- Determine what the next of our project, make more improvement?
- Prepare for apper sharing: Mozilla's rr (**NOT for next week**)


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

## Last week's plan

Replay machine

## This week's work

1. add the ability to read from a file
2. understanding syscord and study syscall

## Next week's plan 

Replay machine & study syscall

