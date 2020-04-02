# 【转】ARM aarch64汇编学习笔记（二）：ARM DS-5模拟器安装和使用

​                                          原创                                                                                                                                              [Hober_yao](https://me.csdn.net/yhb1047818384)                     最后发布于2018-07-14 19:05:45                                         阅读数 2167                                                                                                                             收藏                                                          

​                                                                 展开                           

原文链接：https://blog.csdn.net/yhb1047818384/article/details/81045564###          

工欲善其事，必先利其器。 使用Qemu 虽然可以进行模拟开发，但在Qemu调试汇编有一些困难。
 DS-5 (即ARM Development Studio 5) ，是一款针对 ARM 支持的 Linux 和 Android 平台的全面的端到端软件开发工具套件。

## DS-5 安装

1. 从官网选择一个ARM DS-5版本进行下载
    ![这里写图片描述](https://img-blog.csdn.net/20180714172801189?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我选择的版本是5.26.2， 已经支持Arm v8了。
 下载完成后，解压， 点击setup.exe 进行安装， 安装完成后需要添加license, 否则项目无法编译。
 ![这里写图片描述](https://img-blog.csdn.net/20180714173012166?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

license添加完成后既可以正常使用。

\##使用DS-5 创建程序

1. 首先新建一个空的C project,  输入project name, 选择tool chains为Arm compiler 6。
    ![这里写图片描述](https://img-blog.csdn.net/20180714172332140?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
2. 右击刚才新建的project, 添加source file
    ![这里写图片描述](https://img-blog.csdn.net/20180714172606743?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

新增一个C文件 main.c 和一个汇编文件 asm_add.s。写一个很简单的a + b =c的程序。代码的核心部分使用汇编实现，C程序主要是入口以及检查结果的准确性。

主程序：
 ![这里写图片描述](https://img-blog.csdn.net/20180714180325171?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

汇编部分：
 ![这里写图片描述](https://img-blog.csdn.net/20180714180407736?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. 代码编译
    在编译之前需要预先做一些配置， 右击项目， 点击属性，选择C/ C++ build
    ![这里写图片描述](https://img-blog.csdn.net/2018071418055380?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

选择设置， 将All Tools settings下的target CPU更改为arm v8：
 ![这里写图片描述](https://img-blog.csdn.net/20180714180730920?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

将ARM linker6 中的Image_layout 改为如下配置：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120211549398.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=,size_16,color_FFFFFF,t_70)

应用这些修改后， 右击项目， 选择build project：
 ![这里写图片描述](https://img-blog.csdn.net/20180714180953754?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如果编译成功， 会在Debug目录下生成object 和 axf文件。

1. DEBUG 设置
    选择run-> debug configuration
    ![这里写图片描述](https://img-blog.csdn.net/20180714184933316?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

右击DS-5 debugger， 新建debug 配置
 connection 选择 arm v8-a
 ![这里写图片描述](https://img-blog.csdn.net/20180714185634531?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

files选择刚刚编译出来的axf文件：
 ![这里写图片描述](https://img-blog.csdn.net/20180714185200741?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

debugger选择从main开始， 然后apply， 再然后点击debug
 ![这里写图片描述](https://img-blog.csdn.net/20180714185645631?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. 运行
    点击debug 后，生成如下的界面
    ![这里写图片描述](https://img-blog.csdn.net/20180714185845727?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

点击左上角的开始按钮， 会从左下角的光标位置开始运行， 右上角可以看程序的寄存器河内存信息， 右下角可以看程序的运行结果。

选择单步调试，当运行到汇编部分时，查看寄存器状态
 ![这里写图片描述](https://img-blog.csdn.net/20180714190203129?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看到，当运行到ADD w0, w0, w1那一步时，core寄存器的x0, x1已经变成了预设的2和3.

最终结果：
 

![这里写图片描述](https://img-blog.csdn.net/20180714190343564?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3loYjEwNDc4MTgzODQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

