# 系统镜像
使用Rockpi 4b的Armbian镜像可启动，配合armbian_tn3399_v3_overlays.dts解决部分设备无法驱动问题。

Armbian: https://www.armbian.com/rockpi4/

```
armbian-add-overlay armbian_tn3399_v3_overlays.dts
```

# 制作idbloader
Rockpi 4b的Armbian镜像，内存频率设置在933MHz，TN3399_V3使用会偶现内存错误，可替换为800MHz。
```
git clone https://github.com/rockchip-linux/rkbin.git
./tools/mkimage -n rk3399 -T rksd -d bin/rk33/rk3399_ddr_800MHz_v1.27.bin idbloader.img
cat ./bin/rk33/rk3399_miniloader_ >> idbloader.img
```

# 替换Armbian预编译镜像的idbloader:
```
dd if=idbloader.img of=Armbian_22.11.1_Rockpi-4b_bullseye_current_5.15.80.img bs=512 seek=64 conv=notrunc
```

# 替换Install to Emmc的idbloader:
```
cp idbloader.img /usr/lib/linux-u-boot-current-rockpi-4b_22.11.1_arm64/idbloader.bin
armbian-install
```

# Armbian内核构建
```
./compile.sh BOARD=rockpi-4b BRANCH=current RELEASE=jammy BUILD_MINIMAL=no BUILD_DESKTOP=no BUILD_ONLY=kernel KERNEL_CONFIGURE=no
```

```
CONFIG_ANDROID=y
CONFIG_ANDROID_BINDER_IPC=y
CONFIG_ANDROID_BINDERFS=y
CONFIG_ANDROID_BINDER_DEVICES=""
```