# 使用 Diskpart 对硬盘分区进行调整

## 文档说明

新建两个分区，按 C 盘与 D 盘的顺序分别向其中写入同一个镜像，就可组成双系统。

若硬盘中是 C D 两个分区中的系统组成双系统，系统引导界面将会分别显示两个系统分别位于某个分卷，其中卷序号较小的引导对应的是 C  盘中系统，卷序号较大的引导对应的是 D 盘中的系统。

进行测试时应当选择卷序号较大的启动以开启用于测试环境的系统。

本教程将说明如何在测试完成后将 D 盘中删除、删除后调整分区大小、新建分区并且修复引导。

在此案例中，操作前 C 盘容量为 60G，D 盘 50G。本次操作后将会将 C 盘调整为 100G，D 盘为当前硬盘剩余所有空间。

## 准备
无需额外工具，可备一个带 win10 镜像的启动盘做备用。

## 步骤

1. 打开 Diskpart 工具

    测试完成时，C 盘中的系统应是还没有启动过的状态，开启时会有初始设置界面，我们需要使用的 Diskpart 工具就需要在这个界面上打开。

    此时我们只需要在启动电脑时，选择卷序号较小的引导启动系统，在开机后语音提示停止后，按 shift + F10 打开命令提示符，输入 diskpart，并回车。

2. 删除分区

    分别键入以下命令并按回车以删除分区：

    list disk    （列出电脑中的所有磁盘）

    select disk 0 (选中指定序号的磁盘，其中 disk 后面接着的的数字 0 为系统所在磁盘的序号)

    list partition    （列出所选磁盘中所有分区）

    select partition 5 （选中指定序号的分区，其中 partition 后面接着的数字为 D 盘所在系统的分区序号）

    delete partition

    ![删除分区操作图示](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/modify-disk-partition-with-diskpart/deletePartition.png)

3. 调整分区大小

    list partition 

    select partition  4  (选中 C 盘所在的分区)

    extend size=40960  (将选中的分区扩大 40960M)

    ![调整分区大小操作图示](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/modify-disk-partition-with-diskpart/extendPartition.png)

4. 新建分区

    list disk 0 (后面加要新建分区的磁盘)

    create partition primary (为所有剩余空间创建一个主分区)

    format fs=ntfs quick (执行快速格式化，分区格式为 ntfs)

    assign letter=D    (将此分区盘符设为 D)

    ![新建分区操作图示](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/modify-disk-partition-with-diskpart/createPartition.png)

    ![修改盘符操作图示](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/modify-disk-partition-with-diskpart/assignLetter.png)

    可执行 list partition 再次查看分区列表，以验证是否执行成功。至此，硬盘分区相关的操作已经全部完成。
    
    操作完成后输入 exit 并回车退出 diskpart 工具。

5. 修复引导

    引用其他教程，可参照 [使用 bcdboot 修复引导](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/repair-boot-menu-with-bcdboot.md)

    此处操作与教程不同的地方为，不需要借助启动 U 盘，只需要直接在命令提示符中输入命令即可操作。

    ![修复引导操作图示 1](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/modify-disk-partition-with-diskpart/bcdboot.png)

    ![修复引导操作图示 2](https://github.com/oh1h0ney/Git-Book-Library/blob/master/Windows/modify-disk-partition-with-diskpart/bcdboot1.png)

6. 检测

    在所有操作完成后，可以将电脑重启，观察是否可以用默认引导进入 C 盘的系统初始设置界面。

    在初始设置界面中没有任何按键可以进行重启或关机，强制关机后再次关机也将会是继续进入到关机前的界面，此时应再次按 shift + F10 ，打开命令提示符，输入如下界面以关机：

        shutdown -s -t 0

    此命令代表 延时 0 秒执行静默关机操作。

    此教程完结。