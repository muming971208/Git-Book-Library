# 在 Win10 中禁用笔记本自带的键盘

# 前记：

你们别问我为什么要禁用笔记本自带的键盘，有人就是要禁用，然后在上面放一个外接键盘。。

## 禁用方法

其实禁用自带的键盘很简单，只需要一个命令。

打开 PowerShell，输入以下命令：

`sc.exe config i8042prt start= disabled`

重启电脑，你就会惊奇地发现，笔记本自带的键盘已经用不了了。

## 恢复方法

想要恢复？ 也很简单，一样的步骤，把命令换一下就好了，重新启用键盘的命令如下：

`sc.exe config i8042prt start= auto`