# Linux 使用

!!! question "本文是否适合你"
    如果你的目的是入门 Linux，我们极力推荐 USTC LUG 撰写的非常强大的 [Linux 101](https://101.ustclug.org/)

    其他非常有用的资源包括 [The missing semester](https://missing-semester-cn.github.io/2020/command-line/) 和 [Command Line Cheetsheet](https://threenine.co.uk/download/1846/)

    接下来我们将只讨论他们漏掉的部分

## 在服务器和本地之间传输数据

**选项 1**：使用 `scp`，格式和 `cp` 一致。

```
scp meow@example.com:~/foo.cpp ./bar.cpp
scp -r ./dir meow@1.2.3.4:
```

如果需要改变端口，请使用 SSH 配置文件 `~/.ssh/config` 添加 Host

**选项 2**：使用 `rsync`。rsync 会进行滚动哈希，不会重复传输已有的内容

通常的使用模式是：

```
rsync -av --info=progress2
```

## 产生/检查校验码

Linux 常用的 Hash 函数包括 md5, sha1 和 sha2。考虑目前的碰撞可能和执行速度，我们推荐使用 sha256, 对应的命令 `sha256sum` 包含于 GNU coreutils，预装于绝大多数发行版。

当然，如果你看到了其他后缀名的校验码文件，应该是用对应的程序进行检验。命令格式和 `sha256sum` 相同。

```
# 产生校验码
sha256sum ./file > ./file.sha256
# 对一个目录中所有的文件都产生校验码
find ./dir -type f -exec sha256sum "{}" + > ./dir.sha256

# 进行检验
sha256sum -c ./file.sha256 ./dir.sha256
```

## 需要开机执行一个命令 / 自动重启一个进程

通常把这种东西称为服务，配置服务的方式取决于系统的 [init](/dict/linux#init)。在 2018 年以后的大多数发行版大多使用 `systemd` 完成这一工作。运行 `systemctl status`，如果能找到命令，显示所有的服务的状态，说明系统运行在 systemd 上。按下 `q` 可以关闭服务状态的显示。

TL;DR: 通过创建 `/etc/systemd/system/[服务名].service` 来添加一个 `Service Unit`。例如一个执行 `/home/meow/server` 二进制，并在异常退出之后自动重启的服务可以写作：

??? note "/etc/systemd/system/meow-server.service"
   ```
   [Unit]
   Description=Meow Server
   After=network.target

   [Service]
   ExecStart=/home/meow/server --flag1 --flag2 --flag3
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

之后执行：
```
# 启动这一服务
systemctl start meow-server.service

# 检查状态
systemctl status meow-server.service

# 检查日志（标准、异常输出）
journalctl -xe -u meow-server.service

# 开机启动
systemctl enable meow-server.service
```

See: [ArchLinux Wiki: systemd](https://wiki.archlinux.org/index.php/systemd)

## 需要定期执行一个命令

传统的方式是使用 corn，但是在有 systemd 作为 init 的系统上，我们同样推荐使用 systemd 完成这一工作。

TL;DR: Timer Unit 需要配套同名的 Service Unit，并且 Service Unit 必须设置为 Type=oneshot。Timer Unit 的配置形如 `/etc/systemd/system/[服务名].timer`

See: [ArchLinux Wiki: systemd/Timers](https://wiki.archlinux.org/index.php/Systemd/Timers)
