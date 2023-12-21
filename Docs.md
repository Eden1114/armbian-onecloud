# 从零开始的玩客云armbian使用手册

## 为什么要写这个使用手册？

目前网上的玩客云刷armbian教程很多，但是并没有一个系统性和条例，
很多人在使用过程中遇到问题，但是没有一个标准的解决方案。
希望这个repo可以让大家可以从零开始，搭建自己的armbian系统，这样遇到问题也更好解决。

## 构建armbian的镜像

这个repo主要是用github actions来构建armbian镜像，这里首先给感谢这个repo的作者hyzitc。
通常来说你需要fork`https://github.com/hzyitc/armbian-onecloud`这个repo。

然后clone你自己的repo：
```bash
git clone git@github.com:your_name/armbian-onecloud
```

然后编辑`.github/workflows/ci.yml`文件：

这里可以修改`on:`配置actions执行的触发条件，我选择使用了push，后面就使用push提交就可以了。

```yaml
on: push

env:
  SCRIPT_REPO: ${{ github.repository }}
  SCRIPT_REF: ${{ github.ref_name }}
  UBOOT_REPO: hzyitc/u-boot-onecloud
  UBOOT_RELEASE: latest
  UBOOT_BURNIMG: eMMC.burn.img
  ARMBIAN_REPO: armbian/build
  ARMBIAN_REF: v23.11 # armbian的版本
  PATCHES: 4077,5076
  PATCHES_DISABLED: 5082
```

对于内核有定制需求的，也可以fork armbian官方的build repo(`github.com/armbian/build`)
，修改配置后，更改这里的env.armbian_repo配置项。

修改完配置后，通过push来执行actions

```bash
git commit -m "update"
git push origin
```

之后在github的actions下，可以看到构建的结果，一般需要等1个小时以上。可有在relase下载构建好的镜像。
以burn.img结尾的镜像可以使用usb burning tools（注意版本）直接刷入。

其余的镜像则需要做一些处理

## 设置启动脚本

参考 `https://github.com/hzyitc/armbian-onecloud/issues/13`

