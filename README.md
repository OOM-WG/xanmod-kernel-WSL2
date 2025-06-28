#### 适用于 WSL2 的非官方内核。fork自 [Locietta/xanmod-kernel-WSL2](https://github.com/Locietta/xanmod-kernel-WSL2)，进行了部分修改。  

#### Xanmod 内核相对于原版内核的增强之处（AI收集）
- 优化了核心和进程调度、负载均衡、缓存、虚拟内存管理、CPU频率调节器（针对重型工作负载）
- 增强块层以实现高IOPS吞吐量
- 使用LLVM Clang的高级内核构建优化
- 使用ORC Unwinder改进调试
- 使用Google的多代LRU框架进行高级内存管理
- 支持实时内核（PREEMPT_RT）
- 支持进程调度器如sched_ext (SCX)
- AMD 3D V-Cache优化器驱动
- Cloudflare的TCP折叠处理（提高网络性能）
- Google的BBRv3 TCP拥塞控制
- Netfilter增强（NAT和数据包处理）
- NT同步原语仿真驱动
- PCIe ACS Override（提高硬件兼容性）
- Graysky的额外CPU选项
- 部分Clear Linux补丁集
- Android Binder IPC驱动（用于Waydroid）

#### 本fork相对于上游仓库的修改：
- 在 MAIN 分支添加了更多 zram 压缩算法
- 增加了针对 raptorlake 和 alderlake 处理器的编译
- 停止了自动工作流，改为通过 star 或手动触发

---

#### 根据上游的 GPL-2.0 许可证，要做到：
- 保留原有 版权和许可声明
- 提供源代码
- 明确声明修改

---

<br>
<br>

# xanmod-kernel-WSL2
![Xanmod MAIN](https://github.com/OOM-WG/xanmod-kernel-WSL2/actions/workflows/MAIN.yml/badge.svg?branch=main)
![Xanmod LTS](https://github.com/OOM-WG/xanmod-kernel-WSL2/actions/workflows/LTS.yml/badge.svg?branch=main)
![](https://img.shields.io/github/license/OOM-WG/xanmod-kernel-WSL2)
![version](https://badgen.net/github/release/OOM-WG/xanmod-kernel-WSL2)

一个非官方的 [XanMod](https://gitlab.com/xanmod/linux) 内核移植，为 **WSL2** 集成了 [dxgkrnl](https://github.com/microsoft/WSL2-Linux-Kernel/tree/linux-msft-wsl-6.6.y/drivers/hv/dxgkrnl) 驱动，并由 [clang](https://clang.llvm.org/) 编译器开启 ThinLTO 编译。

本仓库包含一个自动化的 **GitHub Action** 工作流，用于构建和发布 WSL 内核镜像。它每天检查上游是否有新版本可用，并相应地触发构建和发布过程。 

我们目前同时发布最新的稳定版 (MAIN) 和长期支持版 (LTS) 的 Xanmod 内核，LTS 内核构建版本会带有额外的 `-lts` 后缀。

## 使用方法

### 手动安装

*   从 [releases](https://github.com/OOM-WG/xanmod-kernel-WSL2/releases) 下载内核镜像。
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

### （不支持）~~通过 Scoop 安装~~

### 更新内核

要更新 WSL2 的内核，您可以手动用新版本替换掉旧的内核镜像。

**注意：** 要使内核更新生效，您必须重启 WSL2（即运行 `wsl --shutdown` 并打开一个新的 WSL2 实例）。

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

如果您想改进这个仓库，欢迎（向上游）提交 PR。
