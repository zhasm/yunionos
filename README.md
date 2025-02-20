Cloudpods PXE／ISO ROM scripts
===============================

## 使用方法

### 编译 rootfs

rootfs 使用 [buildroot](https://buildroot.org/) 工具进行编译。

为了方便快速制作流程，使用 docker 进行编译。

使用下面的命令制作 buildroot 的 docker 镜像：

```bash
# 制作 buildroot docker 镜像
$ make buildroot-image
```

有了 buildroot 的镜像后，使用该镜像启动容器编译 rootfs，命令如下：

```bash
# 制作 rootfs
$ make docker-buildroot

# 查看制作好的镜像
$ ls ./output/rootfs.tar
```

### Bundle 所有文件

把编译好的 rootfs ，kernel 和自定义的脚本以及二进制工具捆绑到一块。

如果本地没有内核，先使用下面命令下载内核：

```bash
$ make download-kernel-rpm

$ ls kernel*
kernel-ml-5.12.9-1.el7.elrepo.x86_64.rpm
```

然后执行下面的命令进行 bundle：

```bash
$ make docker-bundle

# 生成的文件会在 ./output_bundle
$ ls output_bundle
baremetal_prepare         bootx64.efi  intermediate  ldlinux.c32  libcom32.c32  menu.c32
baremetal_prepare.tar.gz  chain.c32    isolinux.bin  ldlinux.e32  libutil.c32   pxelinux.0
bootia32.efi              initramfs    kernel        ldlinux.e64  lpxelinux.0
```

### 将 bundle 的文件做成 RPM

```bash
$ make docker-make-rpm

$ ls output_bundle/x86_64/
baremetal-pxerom-1.1.0-21060312.x86_64.rpm
```

### 更新 pxelinux 固件

pxelinux 目录下面是从 syslinux 拷贝过来的 pxe 启动固件，可以使用以下的命令更新：

```bash
$ make pxelinux-update
```
