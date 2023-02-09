---
title: C语言中的结构体对齐
tags: 
- 编程
- c
---

## 例子

```c
#pragma pack(2)
typedef struct _st
{
    uint8_t a;
    uint32_t b;
    uint16_t c;
}__attribute__((aligned(16))) st;
#pragma pack()
```

`hello`结构体的大小为 **16**, 因为c语言中结构体的大小遵循三大原则.

## 原则

1. 成员在其大小倍数的位置访问速度最快
2. 使用#pragma pack(m)来限制内部紧凑性
3. 使用__attribute__((aligned(n)))来限制外部对齐

## 推理

1. 仅考虑原则1， 则`a`在`0`, `b`在`4`,  `c`在`8`; 整体大小应该为`10`;
2. 结合原则2， `pack(2)`表示按照`2`的倍数进行处理， 尽可能**内部**紧凑。则`a`在`0`, `b`在`2`, `c`在`6`; 整体大小应该为`8`;
3. 结合原则3， `aligned(16)`表示结构整体应当按16**外部**对齐, 则偏移不变，但是整体大小应该为`16`;

## 答疑

1. 如果不设置, pack和aligned是多少？

答：与编译器和目标架构有关。

2. {uint8_t, uint16_t} , pack(8), aligned(1) 该配置下为什么大小为`4`, 不应该为`8`吗？

答：pack(8)约束太宽松。成员最大为4字节，按照原则1已经满足约束条件。

2. {uint8_t, uint16_t, uint8_t} , pack(2), aligned(1) 该配置下为什么大小为`6`, 不应该为`5`吗？

答：原因在于最后存在一个补足位；该补足位不是因为pack(2), pack是为了紧凑，不补足既满足pack(2)又紧凑不是更好，同时aligned(1)保证外部对此没有要求。补足是因为第二位的`uint16_t`和原则1，对于单个`st`其大小为5当然没问题。原因在于原则1，当存在`st[10]`这种数组时，访问第一个元素没问题符合原则1，但是访问第二个元素的第二个成员就会出现奇地址，显然不满足原则1，因此需要补全一个字节。