# 裸机相关的设置
##　&nbsp;　(1)im6ull裸机LED stm32写法
### 一、stm32寄存器结构体详解
#### &nbsp;&nbsp; 使用一个结构体将所有外设的所有寄存器都放在一起
### 二、修改驱动
1. 添加清除bss端代码
```c
.global _strat
.global _bss_start
_bss_start:
    .word __bss_start
.global _bss_end
_bss_end:
    .word __bss_end
_start:
    mrs r0, cpsr /* 读取cpsr到r0*/
    bic r0, r0, #0x1f /*清除cpsr的bit4-0*/
    orr r0, r0, #0x13 /*使用SVC模式*/
    msr cpsr, r0      /*将r0写入到cpsr/
    /*清除BSS段*/
    ldr r0, _bss_start   /*r0存放的是bss_start 的address*/
    ldr r1, _bss_end /*r1存放的是bss_end 的address*/
    mov r2, #0
bss_loop:
    stmia r0!, {r2} //把r2寄存器里的0 存入r0寄存器的的地址里  r0+1
    cmp r0, r1 //比较r0和r1里面的值
    //如果不等于的话就需要继续跳回loop
    ble bss_loop

    //设置SP指针
    ldr sp, =0x80200000
    b main //跳转到c语言的首地址
    

````   
2. 添加寄存器结构体
   要注意寄存器的连续性
3. 修改驱动

##　&nbsp;　(2)im6ull裸机LED sdk写法
###一、官方SDK移植
####1、新建cc.h文件
 
####2、移植文件
   需要移植的文件fsl_commom
