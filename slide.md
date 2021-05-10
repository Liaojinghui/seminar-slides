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

# Wenxuan

## Last Week's work

- Evaluation of non-concurrency bugs
- Revise the paper
- \[EXTRA\] Handle PMU interrupt in kernel

---

## Handle PMU interrupt in kernel

ETM output should be moved out from the buffer periodically. That means we
should handle the interrupt in the kernel.

Use `request_irq()` to register a user-defined handler for a certain interrupt.

```c
int request_irq(unsigned int irq, irq_handler_t handler,
unsigned long irqflags, const char *devname, void *dev_id)
```

---

## A carefully look into `request_irq`

```c
int request_irq(unsigned int irq, irq_handler_t handler,
unsigned long irqflags, const char *devname, void *dev_id)
```

| argument | meaning                                     |
| -------- | ------------------------------------------- |
| irq      | the interrupt id                            |
| handler  | a function pointer, `int (*func_ptr)(void)` |
| devname  | a string, the name of your device           |
| dev_id   | a magic number                              |

### dev_id
`void *dev_id` is used to identify the device handling the interrupt. For
example, if you want to delete the interrupt handler:

```c
void remove_irq(unsigned int irq, void *dev_id);
```

---

## Handle PMU interrupt
```
int request_irq(unsigned int irq, irq_handler_t handler,
unsigned long irqflags, const char *devname, void *dev_id)
```

\vspace{20pt}

It should have been easy if we wrote a kernel module like this:

```c
static const unsigned int pmu_irqs[]
    = { PMI_A72_core0, PMI_A72_core1, ..., };

for (irq_i = 0; irq_i < 6; irq_i++) {
    request_irq(pmu_irqs[irq_i], pmu_irq_handler, 
    IRQF_DEFAULT, "my_handler", (void*)pmu_irq_handler);
}
```

---

## Linux default handler

Linux uses the PMU as performance counter (`perf` tools), so it already has a
handler for PMIs.

You can see them with `cat /proc/interrupts`

![](images/interrupts.png)

And... They are set to **NON_SHAREABLE**.

---

## Replace the default handler with ours

### PLAN A
```c
void remove_irq(unsigned int irq, void *dev_id)
```
We know the `irq`, but I don't know the `dev_id`! By checking the linux source,
I found that the `dev_id` is a memory reference dynamically generated when the
linux boots!

. . .

### PLAN B
Change the default handler by modifing the linux kernel. Stop registering it, or
add SHAREABLE flags to it.

---

## Logic interrupt ID vs Hardware interrupt ID

I found the result is still odd...

![](images/afterhandler.png)

There exists another **mapping** between hardware interrupt id and kernel
interrupt id! (It's not simply adding a number or something. It's a complex map)

The interrupt id for PMI in Linux is 36, 37, 38, 39, 40, 41.

---

Tips on writing and revising a paper

---

## Tips on writing and revising a paper

- How to quickly check the grammar errors in a paper.
- How to use Git to sync paper writing and sending emails automatically.

---

## How to quickly check grammar errors

### Grammarly

[Grammarly](https://www.grammarly.com/) is a great tool to check grammar errors.
However, it dislikes passive voice, we should not trust every results it gives
out.

. . .

### Copy text from PDF

Copy and paste in a PDF file is painful! You have to handle break-lines,
dislocated characters, and so many other things!

To quickly copy lines in paper, first change the first line in LaTeX

```latex
\documentclass[manuscript, anonymous]{acmart}
```

and use the command line tool `pdftotext`.

---

## How to use Git and send emails automatically

Use the Git Action I create [=>GitAction](https://github.com/COMPASS-ReplayB/CCS21-ETMBugAnalysis/blob/master/.github/workflows/blank.yml)

---

What makes a paper

---

## What makes a paper

We fill the email inbox with 65 mails in one day.

![](images/mail.png)

---

## What makes a paper

We push 381 versions of paper to Git.

![](images/glog.png)

---

## What makes a paper

### **04:40:36** (Wed Apr 21)

Yiming made the `nightest' edit in a day

### **08:20:35** (Tue Apr 20)

Haonan made the `earliest' edit in a day

---

## What makes a paper

| Name    | \# of commits | add lines | delete lines |
| ------- | ------------- | --------- | ------------ |
| Yiming  | 115           | +5455     | -2877        |
| Haonan  | 110           | +3858     | -3013        |
| Yuxin   | 26            | +763      | -593         |
| Wenxuan | 35            | +896      | -1590        |
| Xueying | 20            | +760      | -461         |
| Fengwei | 21            | +144      | -129         |
| Zhenyu  | 40            | +2023     | -949         |

---

## Special Thanks to Zhenyu

6 meetings. Add up to 22 hours.

| Meeting Time   | Apr 28 | Apr 29 | Apr 30 | May 1 | May 2 | May 4 |
| -------------- | ------ | ------ | ------ | ----- | ----- | ----- |
| Meeting Length | 5:09   | 3:08   | 4:28   | 3:02  | 4:50  | 1:33  |

![](images/vedio.png){height=40%}

---

## Special Thanks to Jingting, Yihao

They gave us much advice.

![](images/yihao.png)

## Special Thanks to some one

who gave up the vacation to be with me and supported me in the midst of
depression, loneliness and pain :)

# Xueying

---

## self-examination

1. Didn't work hard enough
2. Be an active learner
3. Be confident

---

## Thanks 

I am extremely grateful to all my teammates and Zhenyu.

I had learned a lot from this experience.
