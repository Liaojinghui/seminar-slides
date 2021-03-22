---
title: "Summary for Last Week"
author: "Haonan Li, Wenxuan Shi, Xueying Zhang"
institute: "COMPASS"
urlcolor: blue
colortheme: "beaver"
date: "March 23, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---
# Haonan

## Plan of Last Week

- Improve the syscall capturing
- Paper writing

---


## Actual Progress

*All are in progress*

- syscall capture: share memory between kernel space with user space
- Paper writing: finsh outline *(for every section)* and section **Intoducton** *(but needs to be rewritten)* 
- Talk with Zhenyu for many times...

---

## Share Memory

see: [Stack Overflow](https://stackoverflow.com/questions/36762974/how-to-use-mmapproc-shared-memory-between-kernel-and-userspace) or my [github](https://github.com/Tert-butyllithium/remapping-driver)

Basic idea: share by **physical address**

### Some Special Techniques:

- Share *file* : `/proc/...`
- Create files by specific their file operation handlers..

---

## Manually Specify File Operation Handlers

```c
static const struct file_operations phymem_info_fops = {
    .owner = THIS_MODULE,
    .open = proc_open_meminfo,
    .read = seq_read,
    .llseek = seq_lseek,
    .release = seq_release};

static const struct file_operations mmap_fops = {
    .owner = THIS_MODULE,
    .mmap = proc_mmap};
```


---

## Share Memory

see: [Stack Overflow](https://stackoverflow.com/questions/36762974/how-to-use-mmapproc-shared-memory-between-kernel-and-userspace) or my [github](https://github.com/Tert-butyllithium/remapping-driver)

Basic idea: share by **physical address**

### Some Special Techniques:

- Share *file* : `/proc/...`
- Create files by specific their function handlers..
- marco `__pa` and `__va` in kernel...


---

## Paper Writing 

### Problem: ``Single Thread"
All of us are waiting for Yiming's work:

- Performance test
- More POCs
- Some works about concurrency bug


---

## Plan for Next Week

- Paper Writing:
	- Write an detaild outline for paper
	- Write a part of **Design**, and **Implementation**, rewrite **introduction**
- Continue to improve syscall capturing, if `sysdig` is really not enough (`25%` overhead in *UnixBench*)
- Take part of Yiming's work: evaluation, more POCs...

# Xueying

---

## Last Week's work

Took a TOEFL test.

---

## Next week's work

Use C++ to run a single assembly code from user input.

# Wenxuan

---

## Last week's plan

- [x] Use C++ to run a single assembly code from user input.

- [ ] Find an algorithm to replace branch instructions in the assembly code.


---

## Run assembly from user input

![](ipc_step1.png){height=90%}

---

## Run assembly from user input

![](ipc_step2.png){height=90%}

---

## Run assembly from user input

![](ipc_step3.png){height=20%}

### Make a function pointer

C style function pointer:

```c
void (*foo)(void) = (void (*)(void)) ptr;
```

In a normal C function, `sp (x29)` and `ra (x30)` are stored in the stack. We currently need to store that, too. But **eventually**, we will come up with an idea to maintain context.

### Invoke the function pointer
And then, try to execute:

```c
foo();
```

---

## Run assembly from user input

![](ipc_step4.png){height=90%}

---

## Run assembly from user input

![](ipc_step5.png){height=90%}

---

## Run assembly from user input

![](ipc_step6.png){height=90%}

---

## Memory Allocation

`mprotect()` can only change the permission of a certain **page** (coarse-grained). A better approach is to preserve a section in the memory at compile time using a **link script**.

![](ipc_step7.png){height=90%}

---

## One more thing: Switch from SVN to GIT

::: columns

:::: column

Looks well on mobile device.

![](email_report_iPhone.JPEG){height=70%}

::::

:::: column

Looks great on desktop device.

*(Outlook, Mac Mail App)*

![](email_report_screenshot.png){height=80%}

::::

:::

---

## Next week's plan

- [ ] Create a new subprocess, find a way to overwrite its memory.

- [ ] Help writing the paper.

- [ ] Find an algorithm to replace branch instructions in the assembly code.
