---
title: 内存屏障
date: 2022-02-26 08:00:00
tags: c
---

## DMB

DMB, 全称Data Memory Barrier(数据内存屏障)。先看ARMv7-M Architecture Reference Manual手册中的说明。

> Data Memory Barrier acts as a memory barrier. It ensures that all explicit memory accesses that appear in
program order before the DMB instruction are observed before any explicit memory accesses that appear in
program order after the DMB instruction. It does not affect the ordering of any other instructions executing on
the processor.

其中Memory Access的[定义](https://www.vocabulary.com/dictionary/memory%20access#:~:text=Definitions%20of%20memory%20access,synonyms%3A%20access)为读写，既包括读也包括写。

>  (computer science) the operation of reading or writing stored information

因此，DMB的作用为保证DMB前的内存读写一定发生在DMB后，但是并**不保证**其它指令的执行顺序。

## DSB

> Data Synchronization Barrier acts as a special kind of memory barrier. No instruction in program order after this
instruction can execute until this instruction completes. This instruction completes only when both:

> • Any explicit memory access made before this instruction is complete

> • The side-effects of any SCS access that performs a context-altering operation are visible.

DSB与DMB类似，作用更加明显，保证DSB前的所有指令在DSB之前都执行。

## ISB

> Instruction Synchronization Barrier flushes the pipeline in the processor, so that all instructions following the ISB
are fetched from cache or memory after the instruction has completed. It ensures that the effects of context altering
operations, such as those resulting from read or write accesses to the System Control Space (SCS), that completed
before the ISB instruction are visible to the instructions fetched after the ISB.

ISB与DSB类型，不仅保证前面的指令执行，而且清空流水线，使得后续执行需要重新从内存或者Cache中取。该指令并**不清空**Cache。


## 参考

- ARM®v7-M Architecture Reference Manual
- [A answer from stackoverflow](https://stackoverflow.com/questions/15491751/real-life-use-cases-of-barriers-dsb-dmb-isb-in-arm)