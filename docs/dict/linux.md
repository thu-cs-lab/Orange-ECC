# Linux 使用相关

### ELF

`abbr.` = Executable and Linkable Format

直译过来就是“可执行、可链接文件格式”。在 Linux 中大部分的可执行二进制，以及编译得到的 Object file 都是 ELF 格式的。

Also see: 链接 (Linking) 和动态链接 (Dynamic linking, ld.so)

### `ar` / `tar` / `gz` / `bz2` / `zstd` ...

这些是不同的常见的打包 (Archive) 格式。对应的命令、常用文件名和常用用途如下：

- `foo.{gz, bz2, xz, zstd}` / `gzip, bzip2, xz, zstd` : 四种比较常见的通用压缩文件格式
- `foo.tar` / `tar` : 将一系列文件不加压缩地打包的最常见形式
- `foo.a` / `ar` : 将一系列 ELF Object 打包为静态链接库
- `foo.cpio` / `cpio` : initramfs 常用的格式
- `foo.squashfs` / `mksquashfs & mount` : 可以只读 Mount 的文件系统打包

??? note "`tar.___` = tar + 压缩"
    - `foo.tar.gz = foo.tgz` / `tar + gzip` 或者 `tar -z` : 等于 gzip 压缩的 tar，也就是 `tar -c ./file | gzip --stdout > file.tgz`
    - `foo.tar.bz2 = foo.tbz2` / `tar + bzip2` 或者 `tar -j` : 同理
    - `foo.tar.xz= foo.txz` / `tar + xz` 或者 `tar -j` : 同理

### init

指类 Unix 系统中 PID 1 的特殊进程，是内核启动的第一个进程，负责进行系统初始化以及状态维持。例如 init 系统需要负责启动 sshd 进程，并在其异常退出之后自动重启，以维持稳定的 SSH 服务。

As of 2021，多数主流 Linux 发行版使用 systemd 作为 init 系统。
