# 1 文件结构

BSP中的SylixOS文件夹：

- bsp文件夹：主要包括系统启动的程序框架代码，有汇编、内存映射、BSP参数配置，编译后会生成符号表文件

- driver文件夹：运行操作系统时需要用到的底层硬件驱动。

- user文件夹：main.c 在系统启动后创建一个shell终端。

  ![image-20191209164507479](../TyporaImage/image-20191209164507479.png)

# 2 功能

​		为操作系统和上层应用提供一个与硬件无关的软件平台

- 上电时的硬件初始化
- 为操作系统访问硬件驱动程序提供支持
- 集成操作系统所需的软件模块（硬件相关和硬件无关）

**最下系统**：

- 满足在单板上启动操作系统
- 能输出调试信息

# 3 输出文件

**注意查看console输出的内容。**

*.elf:

*.bin:脱掉衣服的ELF文件

*.size

![image-20200328172010037](TyporaImage/BSP开发.assets/image-20200328172010037.png)

## 3.1 window下的反汇编命令

![image-20200328164617589](TyporaImage/BSP开发.assets/image-20200328164617589.png)

## 3.2 文件介绍

1. config.ld:内存布局

   编译之后生成config.lds

2. SylixOSBSP.ld

3. config.h

4. Makefile

# 4 Base

**在bsp中上，层应用如何调用到base静态库中（libsylixos）的函数 ？**

答：  通过符号表 ：所谓符号表就是一些函数名或者全局变量。

## 4.1 符号表导出脚本

文件：baseMini2440\libsylixos\SylixOS\hosttools\makesymbol\makesybol.bat

执行该脚本，生成symbol.c和symbol.h文件，在bsp中调用symbol.c和symbol.h中实现的函数生成符号表。4.1.1

### 4.1.1 symbol.c

1. 静态符号表

   ```c
   /****************************************[1]***************************************/
   SYMBOL_TABLE_BEGIN					   //[2]				
       SYMBOL_ITEM_FUNC(bb_putchar)       //[3],给结构体LW_STATIC_SYMBOL成员赋值
   SYMBOL_TABLE_END
   /****************************************[2]***************************************/
   //定义了以LW_STATIC_SYMBOL为类型的数组元素。相当于int iArray[] = {1, 2, 3,}
   #define SYMBOL_TABLE_BEGIN LW_STATIC_SYMBOL   _G_symLibSylixOS[] = {	
     									   //[4]				
   #define SYMBOL_TABLE_END };							
   /****************************************[3]***************************************/
   #define SYMBOL_ITEM_FUNC(pcName)                       \
       {   {(void *)0, (void *)0},                        \
           #pcName, (char *)pcName,                       \
           LW_SYMBOL_TEXT                                 \
       },	 
   /****************************************[4]***************************************/
   typedef struct __symbol_dump_list {
       void                *DUMPLIST_pv1;
       void                *DUMPLIST_pv2;
   } __SYMBOL_DUMP_LIST;
   
   typedef struct lw_static_symbol {
       __SYMBOL_DUMP_LIST   LWSSYMBOL_dumplist;
       char                *LWSSYMBOL_pcName;
       char                *LWSSYMBOL_pcAddr;
       int                  LWSSYMBOL_iFlag;
   } LW_STATIC_SYMBOL; 
   ```

   

# 5 Makefile

# 6 预习

![image-20200329113640486](TyporaImage/BSP开发.assets/image-20200329113640486.png)

**深入理解计算机原理**

