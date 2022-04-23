# 自制操作系统笔记

## day01

```C
/*
一些汇编指令：
DB：define byte：往文件中直接写入一个字节的指令，DB指令可以直接写入字符串，汇编语言会自动地查找字符串中每一个字符所对应地编码，然后把他们一个字节一个字节地排列，例如 DB “HELLO”
RESB：reserve byte：RESB 10表示预留多少个字节
;：为汇编语言的注释符号
DW：define word
DD：define double-word
$：汇编语言中的一个预定义符号，等价于当前汇编到的段的当前偏移值
*/
```

