---
title: "Summary for Last Week"
author: "Haonan Li, Wenxuan Shi, Xueying Zhang"
institute: "COMPASS"
urlcolor: blue
colortheme: "beaver"
date: "March 30, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---

# Haonan

## Plan of Last Week

- Paper Writing:
  - Write an detaild outline for paper
  - Write a part of **Design**, and **Implementation**, rewrite **introduction**
- Continue to improve syscall capturing

---

## Actual Progress

- Paper Writing:

  - detaild outline (partially completed)
  - finish desgin for my part (also for may graduation project)

- syscall capture: finish kernel part (_but..._)
- finish some performance test...

---

## Paper Writing

I have spend two and a half days for my graduation report...

### Zhenyu's suggestion

``The graduation report can be written in random words as no one cares.”

``You should focus on your project”

---

## Syscall Capture

::: columns

:::: column

![](arch.pdf){width=100%}

::::

:::: column

\vspace{40pt}

- Three parts: _hook_, _filter_, _and buffer_
- Current challenge: transfer records to **user space**
- Our motivation: sysdig with significant overhead

::::

:::

---

## Add a lock

```cpp
  spin_lock_irq(&small_buf_lock);

  sprintf(small_buf,...);
  write_to_buffer(small_buf, &buf_offset);

  spin_unlock_irq(&small_buf_lock);
```

###

this spin lock hardly affects performance during our test

---

## Performance Test: Environment

- tool: `ab` - Apache HTTP server benchmarking tool
- settings: 500,000 requests with concurrency of 1,000
  - `ab -n 500000 -c 1000 http://localhost`
  - `nginx` default page _Welcome to nginx!_
  - Juno board (all cores up) with Debian GNU/Linux 10

---

## Test result

\small
| | **overall time** | **requests per second** |
| ------------------------------- | ----------------- | ----------------------- |
| **baseline** | 119.122 | 4197.38 |
| **sysdig** | 137.100 (+15.09%) | 3646.98 (-13.11%) |
| **sysdig (without output)** | 132.513 (+11.24%) | 3773.20 (-10.10%) |
| **our capture (without fetch)** | 129.470 (+8.69%) | 3861.90 (-7.99%) |
| **our capture (without write)** | 119.158 (+0.30%) | 4196.10 (-0.30%) |
Table: Test reuslts in different tools

### Two points

- sydig introduce considerable overhead not only due to its saving to file
- our capture is unreasonably slow in writing to buffer

---

## Bottleneck analysis for our capturing system

### What did I do in my `writing_to_buffer`?

a `strncpy` and some addition and subtraction operations

\vspace{10pt}

```cpp
void write_to_buffer(const char *src, int *offset) {
  char *buffer_addr = kernel_memaddr;
  unsigned long str_len = strlen(src) + 1;
  unsigned long str_end = (*offset) + str_len;

  if(str_end > BUF_SIZE){
    *offset = 0;
    str_end = str_len;
  }
  strncpy(buffer_addr + (*offset), src, str_len);
  *offset = str_end;
}
```

---

## Bottleneck analysis (cont.)

- It seems the cause is `strncpy`? But what's the difference between `strncpy` with `sprintf`
- Zhenyu's recommendation: multithread ...
- The only difference: `char small_buf[128]` vs. ` __get_free_pages(GFP_KERNEL, PAGE_ORDER);`
- **!!! Don not trust StackOverflow unconditionally**

---

## Test result (cont.)

\small
| | **overall time** | **requests per second** |
| ------------------------------- | ----------------- | ----------------------- |
| **baseline** | 119.122 | 4197.38 |
| **our capture (without fetch)** | 129.470 (+8.69%) | 3861.90 (-7.99%) |
| **our capture (without write)** | 119.158 (+0.30%) | 4196.10 (-0.30%) |
| **our capture (`buffer[1024]`)** | 120.965 (+1.55%) | 4133.41 (-1.52%) |
Table: Test reuslts in different conditions

### Next problems

- How about `kmalloc` or `vmalloc`?
- Could larger buffer further reduce the overhead?

---

## Plan for Next Week

- Pass the midterm defence _casually_
- Finish my syscall capturing
- Take part of Yiming's work: evaluation, more POCs...

# Wenxuan

---

## Last week's plan

- [ ] Create a new subprocess, find a way to overwrite its memory.

- [ ] Help writing the paper.

---

## What did I do last week?

| Day       | Main                                         |
| --------- | -------------------------------------------- |
| Wednesday | NUS Application Material Preparation         |
| Thursday  | NUS Application                              |
| Friday    | SUSTech Global Interview for NUS Application |
| Saterday  | Courses Deadline                             |
| Sunday    | Two interviews from Tencent and ByteDance    |

---

## Target

### Restore the memory layout: From coredump

After the subprocess is forked, cover its memory layout with coredump.

### Control the execution

Stop, reverse, jump, wander in the binary.

---

## How GDB control its target?

`ptrace()`: read and write text, data, stack, and register of another process.

gdb breakpoint: replace the breakpoint instruction in the text section, trap and send a signal to gdb process (stop the process), and then replace the instruction back.

---

## Next week's plan

- [ ] Use `ptrace()` to create a subprocess restored from coredump.

- [ ] Help writing the paper.

- [ ] Group Project Defence (Week 8).