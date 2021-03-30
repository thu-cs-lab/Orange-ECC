# Vim 使用

!!! question "本文是否适合你"
    如果你的目的是入门 Vim，请首先阅读 Vim 的第一方教程 Vim tutor。使用命令 `vimtutor` 即可打开。

## 如何退出 Vim
`:q`，但是使用 Ctrl-Z 可以将 Vim 像其他进程一样挂起。<del>然后可以使用 kill</del>

See also: [hakluke/how-to-exit-vim](https://github.com/hakluke/how-to-exit-vim)

## 快速注释、取消注释多行代码
**选项1**：直接使用 Block Comment，如果语言支持

**选项2**：使用插件。最常用的是 tcomment。你也许需要使用 [vim-plug](https://github.com/junegunn/vim-plug) 更方便地安装插件。

**选项3**：结合 Visual Block + 多光标。

简单来说，插入注释可以使用 ++ctrl+v++ 并移动光标，选中需要插入注释符号的列，使用 ++shift+i++ 进入多光标模式并输入（这个时候还看不出来，只会更新选中的第一行的位置）。在使用 ESC 退出插入模式的时候，所有选中的行所在位置会插入刚输入的字符串。

删除注释同样可以使用 ++ctrl+v++ 并移动光标，选中多行中的注释符号，然后使用 ++x++ 删除。

## 区域替换
使用 ++shift+v++ 或者 ++ctrl+v++ 选中一个区域之后，按下 ++":"++，显示出的命令 Prompt 会自动填入 `:'<,'>` 表示“在选中区间内执行”（回想全局替换的 `:%`）。之后正常使用 sed 格式 `s/a/b/g` 即可。

## 多 Tab 和分屏

使用 `:tabnew` 或者 `:tabedit PATH/TO/FILE` 添加一个新的 Tab。使用 ++g++ & ++t++ 和 ++g++ & ++T++ 切换到下一个/上一个 Tab。

使用 `:vsp [PATH/TO/FILE]` 和 `:hsp [PATH/TO/FILE]` 垂直/水平切分窗口。不写路径的话会打开当前编辑的文件。使用 ++ctrl+w++ & ++w++ 和 ++ctrl+w++ & ++shift+w++ 切换切分窗口。

使用 `:q` 命令关闭窗口或者 Tab
