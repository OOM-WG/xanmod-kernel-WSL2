#### 适用于 WSL2 的非官方内核。这是一个fork，进行了部分修改。  
* 添加了更多 zram 压缩算法
* 增加了针对 raptorlake 和 alderlake 处理器的编译（如果你的型号不在其中，欢迎提出pr或fork一份）
* 停止了自动工作流，可通过 star 本仓库触发
<br>

---

<br>

# xanmod-kernel-WSL2
![Xanmod MAIN](https://github.com/Locietta/xanmod-kernel-WSL2/actions/workflows/MAIN.yml/badge.svg?branch=main)
![Xanmod LTS](https://github.com/Locietta/xanmod-kernel-WSL2/actions/workflows/LTS.yml/badge.svg?branch=main)
![](https://img.shields.io/github/license/Locietta/xanmod-kernel-WSL2)
![version](https://badgen.net/github/release/Locietta/xanmod-kernel-WSL2)

一个非官方的 [XanMod](https://gitlab.com/xanmod/linux) 内核移植，为 **WSL2** 集成了 [dxgkrnl](https://github.com/microsoft/WSL2-Linux-Kernel/tree/linux-msft-wsl-6.6.y/drivers/hv/dxgkrnl) 驱动，并由 [clang](https://clang.llvm.org/) 编译器开启 ThinLTO 编译。

本仓库包含一个自动化的 **GitHub Action** 工作流，用于构建和发布 WSL 内核镜像。它每天检查上游是否有新版本可用，并相应地触发构建和发布过程。 

我们目前同时发布最新的稳定版 (MAIN) 和长期支持版 (LTS) 的 Xanmod 内核，LTS 内核构建版本会带有额外的 `-lts` 后缀。

## 使用方法

### 手动安装

*   从 [releases](https://github.com/Locietta/xanmod-kernel-WSL2/releases) 下载内核镜像。
*   将其放置在合适的位置。（例如 `D:\.WSL\bzImage`）
*   在当前用户的根目录（`%UserProfile%`）下创建 `.wslconfig` 文件，内容如下：
```ini
[wsl2]
kernel = the\\path\\to\\bzImage
; 例如：
; kernel = D:\\.WSL\\bzImage
;
; 注意，所有的 `\` 都需要用 `\\` 进行转义。
```
*   重启您的 WSL2 以加载新内核，尽情享用吧！

> 关于 `.wslconfig` 的更多信息，请参阅微软官方[文档](https://docs.microsoft.com/zh-cn/windows/wsl/wsl-config#configure-global-options-with-wslconfig)。

### 通过 Scoop 安装

[scoop](https://scoop.sh/) 是一个 Windows 上的命令行安装器。如果您已经安装了 scoop，那么您可以用以下命令来安装此内核：

```bash
scoop bucket add sniffer https://github.com/Locietta/sniffer
scoop install xanmod-WSL2 # xanmod-WSL2-x64v3 的别名

# 其他构建版本
# scoop install xanmod-WSL2-x64v2
# scoop install xanmod-WSL2-skylake
# scoop install xanmod-WSL2-zen3

# LTS 构建版本
# scoop install xanmod-WSL2-lts # xanmod-WSL2-lts-x64v3 的别名
# scoop install xanmod-WSL2-lts-x64v2
# scoop install xanmod-WSL2-lts-x64v3
# scoop install xanmod-WSL2-lts-skylake
# scoop install xanmod-WSL2-lts-zen3
```

Scoop 会自动为您设置 `.wslconfig`，但您仍需重启 WSL2。

### 更新内核

要更新 WSL2 的内核，如果您是通过 scoop 安装的，可以使用 `scoop update *`。或者您可以手动用新版本替换掉旧的内核镜像。

**注意：** 要使内核更新生效，您必须重启 WSL2（即运行 `wsl --shutdown` 并打开一个新的 WSL2 实例）。

> 如果您对我们如何通过 scoop 处理安装和更新感兴趣，请参阅此内核的 [scoop manifest](https://github.com/Locietta/sniffer/blob/master/bucket/xanmod-WSL2.json)。

## 其他

### Systemd

自 WSL 0.67.6 起，兼容[内置的 systemd 支持](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/)；对于旧版 WSL，也兼容 [wsl-distrod](https://github.com/nullpo-head/wsl-distrod)。 

但是 [sorah/subsystemd](https://github.com/sorah/subsystemctl) 和 [arkane-systems/genie](https://github.com/arkane-systems/genie) 会因为修改过的内核版本字符串而拒绝工作（它们要求版本字符串中包含 "microsoft"...）。

我不会将 "microsoft" 添加回版本字符串中（现在已经很长了），因为 "WSL2" 已经足够了，参见 [WSL#423](https://github.com/Microsoft/WSL/issues/423#issuecomment-221627364)。您可以用 `systemd-detect-virt` 来检查，它应该返回 `wsl`。如果有任何更新，我会做出相应更改。

## 致谢

*   Linux 社区提供了这个了不起的操作系统内核。
*   Microsoft 提供了 WSL2 和 dxgkrnl 补丁。
*   XanMod 项目提供了各种优化。

## 贡献

欢迎提交 issue 让我知道 bug、缺失的功能或其他任何事情。

如果您想改进这个仓库，欢迎提交 PR。
