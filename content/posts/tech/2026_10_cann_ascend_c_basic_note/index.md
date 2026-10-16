+++
date = '2026-10-15T13:41:41+08:00'
draft = false
title = '【CANN训练营】Ascend C算子开发基础知识碎碎念'
categories = ["技术"]
tags = ["Ascend C",  "算子开发", "学习笔记"]
series = ["CANN训练营"]

+++

课程目录：
https://gitcode.com/cann/cann-learning-hub/tree/master/tutorials/ascendc_operator_development_light

-> 第二章：Ascend C算子开发基础知识的几个章节

# 02.02_HelloWorld

**notebooke**：

cann-learning-hub/tutorials/ascendc_operator_development_light/02_AscendC_basic/02.02_HelloWorld.ipynb

notebook里的每个代码块就是每次写一部分代码内容写到Sources/02.02/hello_world.asc中（同时附上每一部分的讲解），最后形成完整的helloworld代码，通过bisheng命令编译为二进制文件demo，最后执行，在八个块上打印hello world。

主要流程：（导入头文件） - 编写核函数 - 编写主函数调用 - 编译 - 运行 

**为什么会打印八次？**

因为核函数被8个AI Core并行执行。hello_world 是一个核函数（Kernel），它会被分发到多个AI Core上并行执行。

`hello_world<<<numBlocks, nullptr, stream>>>();  `

这里的 numBlocks = 8 表示：将核函数分配到 8个AI Core​上同时执行,每个AI Core独立运行 hello_world函数体。

**为什么需要 aclrtSynchronizeStream？**
`aclrtSynchronizeStream(stream);`

这行代码的作用是：
让主机端（Host）等待所有AI Core上的核函数执行完毕，确保所有8次打印都完成后，程序才继续执行后续的清理工作。如果没有这行，程序可能会在核函数还没执行完时就退出，导致打印不完整。

**课堂练习**

`AscendC::printf("Block[%lu/%lu] Hi Ascend\n",AscendC::GetBlockIdx(),AscendC::GetBlockNum());`



# 02.03_ascendc_programming_paradigm_and_api_introduction
  
- 基本的矢量编程范式：Ascend C编程范式：CopyIn（数据搬入）、Compute（矢量计算）、CopyOut（结果搬出）
- 基本的矩阵编程范式：CopyIn（数据搬入）、Split（数据分片）、Compute（子矩阵计算）、Aggregate（结果聚合）、CopyOut（结果搬出）
- 复杂的矢量/矩阵编程范式

# 02.04_introduction_to_kernel_functions_based_on_add_operator

**习题：sub算子开发**
sub_kernel.asc


**add作业题**

尝试自己努力，然后很悲惨的失败了...

在csdn上找到了大佬的笔记：https://blog.csdn.net/liupang14159/article/details/166738282?spm=1001.2014.3001.5502

再一次感受到自己有多么差劲，有一次绊倒在代码上，当年的acm也是，现在的算子开发也是..