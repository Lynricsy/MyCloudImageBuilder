# 🚀 MyCloudImageBuilder

> 一个自动化构建定制化 Debian 13 云镜像的工具，集成 Xanmod 内核、Docker、Zsh、现代化 CLI 工具链，开箱即用！

[![Daily Build](https://img.shields.io/github/actions/workflow/status/Lynricsy/MyCloudImageBuilder/daily-build.yml?label=daily%20build&style=flat-square)](https://github.com/Lynricsy/MyCloudImageBuilder/actions)
[![Latest Release](https://img.shields.io/github/v/release/Lynricsy/MyCloudImageBuilder?style=flat-square)](https://github.com/Lynricsy/MyCloudImageBuilder/releases)
[![License](https://img.shields.io/github/license/Lynricsy/MyCloudImageBuilder?style=flat-square)](LICENSE)

---

## ✨ 特性

### 🎯 核心功能

- **📦 完全自动化构建**：基于官方 Debian 13 (Trixie) 云镜像，一键定制
- **🚀 高性能内核**：预装 [Xanmod](https://xanmod.org/) 高性能内核 (x64v3)
- **🐳 Docker 全家桶**：Docker Engine、Compose、Buildx 开箱即用
- **💻 现代化 CLI 工具**：Zsh + Powerlevel10k + eza/bat/fd/ripgrep/btop 等现代工具
- **🌐 网络优化**：BBR + fq_pie 拥塞控制，提升网络性能
- **🧹 自动清理**：移除不必要的日志、缓存，最小化镜像体积
- **📅 每日构建**：GitHub Actions 自动构建并发布最新版本到 Releases

### 🛠️ 技术栈

| 组件     | 版本/技术                   | 说明                   |
| -------- | --------------------------- | ---------------------- |
| 基础镜像 | Debian 13 (Trixie)          | 官方云镜像 (qcow2)     |
| 内核     | Xanmod x64v3                | 高性能、低延迟优化内核 |
| 容器     | Docker CE + Compose v2      | 最新稳定版             |
| Shell    | Zsh + Zim Framework         | Powerlevel10k 主题     |
| CLI 工具 | eza, bat, fd, ripgrep, btop | Rust/Go 现代化替代品   |
| 网络     | BBR + fq_pie                | TCP 拥塞控制优化       |
| 时区     | Asia/Hong_Kong              | 可自定义修改           |

---

## 📥 快速开始

### 方式一：直接下载预构建镜像（推荐）

访问 [Releases](https://github.com/Lynricsy/MyCloudImageBuilder/releases) 页面，下载最新的 `debian-13-generic-amd64-NEXT.qcow2`：

```bash
# 下载最新版本（替换为实际的 release tag）
wget https://github.com/Lynricsy/MyCloudImageBuilder/releases/download/daily-YYYYMMDD/debian-13-generic-amd64-NEXT.qcow2

# 验证校验和
wget https://github.com/Lynricsy/MyCloudImageBuilder/releases/download/daily-YYYYMMDD/SHA256SUMS.txt
sha256sum -c SHA256SUMS.txt
```

### 方式二：本地构建

#### 前置要求

- Linux 系统（推荐 Debian/Ubuntu）
- `libguestfs-tools` 和 `qemu-utils`
- Root 权限（`virt-customize` 需要）

#### 安装依赖

```bash
# Debian/Ubuntu
sudo apt-get update
sudo apt-get install -y libguestfs-tools qemu-utils wget curl

# 设置 libguestfs 后端为 direct（避免权限问题）
export LIBGUESTFS_BACKEND=direct
```

#### 运行构建脚本

```bash
# 克隆仓库
git clone https://github.com/Lynricsy/MyCloudImageBuilder.git
cd MyCloudImageBuilder

# 以 root 权限运行构建脚本
sudo -E bash builder.sh
```

构建完成后，镜像文件输出为 `debian-13-generic-amd64-NEXT.qcow2`。

---

## 🎨 镜像内容详解

### 📦 预装软件包

<details>
<summary>点击展开完整软件列表</summary>

#### 系统工具
- `sudo`, `qemu-guest-agent`, `spice-vdagent`
- `bash-completion`, `unzip`, `wget`, `curl`, `axel`
- `net-tools`, `iputils-ping`, `iputils-arping`, `iputils-tracepath`
- `dnsutils`, `mtr-tiny`, `lldpd`, `lsof`

#### 编辑器与查看器
- `nano`, `vim`, `most`, `less`

#### 压缩工具
- `bzip2`, `zstd`, `p7zip-full`

#### 监控与调试
- `htop`, `btop`, `fastfetch`

#### 现代化 CLI 工具
- `eza` - 替代 `ls`，支持图标和树状显示
- `bat` - 替代 `cat`，语法高亮和分页
- `fd-find` - 替代 `find`，更快更友好
- `ripgrep` - 替代 `grep`，超快文本搜索
- `btop` - 替代 `top`，资源监控神器

#### 开发工具
- `git` + 全局配置
- `tree`, `screen`

#### Shell 增强
- `zsh` + [Zim Framework](https://github.com/zimfw/zimfw)
- [Powerlevel10k](https://github.com/romkatv/powerlevel10k) 主题
- 自定义 aliases（见下方）

#### 容器
- Docker Engine (CE)
- Docker Compose v2
- Docker Buildx
- Containerd

</details>

### ⚙️ 系统配置

| 配置项          | 值                                   | 说明                     |
| --------------- | ------------------------------------ | ------------------------ |
| 时区            | `Asia/Hong_Kong`                     | 可修改脚本自定义         |
| TCP 拥塞控制    | `bbr`                                | 优化网络吞吐             |
| 队列调度器      | `fq_pie`                             | 降低网络延迟             |
| Docker 日志驱动 | `json-file`                          | 最大 3 个文件，每个 10MB |
| Docker 存储驱动 | `overlay2`                           | 推荐的存储后端           |
| Docker 网络池   | `172.18.0.0/16`                      | 避免默认冲突             |
| NTP 服务器      | `time.apple.com`, `time.windows.com` | 时间同步                 |

### 🎯 预配置 Shell Aliases

登录后 Zsh 默认提供以下 aliases：

```bash
ls    → eza --icons --group-directories-first
ll    → eza --icons --group-directories-first -lh
la    → eza --icons --group-directories-first -lah
lt    → eza --icons --group-directories-first --tree
cat   → bat --paging=never --style=plain
catp  → bat --paging=always
find  → fd
grep  → rg
top   → btop
```

### 📝 Git 全局配置

```bash
user.name = Lynricsy
user.email = im@ling.plus
init.defaultBranch = main
core.editor = nano
diff.algorithm = histogram
merge.conflictstyle = diff3
pull.rebase = false
```

以及便捷的 Git aliases：`st`, `co`, `br`, `ci`, `unstage`, `last`, `lg`, `contributors`

---

## 🤖 GitHub Actions 自动化

### 工作流说明

- **触发时间**：每天 UTC 02:30（北京时间 10:30）自动构建
- **手动触发**：在 Actions 页面点击 "Run workflow" 按钮
- **构建流程**：
  1. 下载 Debian 13 基础镜像
  2. 使用 `virt-customize` 定制镜像
  3. 使用 `virt-sparsify` 压缩镜像
  4. 生成 SHA256 校验和
  5. 上传到 GitHub Releases（tag 格式：`daily-YYYYMMDD`）

### 日志分组

脚本集成了 GitHub Actions 日志分组功能，构建过程分为三个可折叠区块：

- **Download base image** - 下载阶段
- **Customize image** - 定制阶段（最耗时）
- **Compress image** - 压缩阶段

在 Actions 界面可以快速定位各阶段的日志和进度。

### Step Summary

构建完成后，会自动生成步骤摘要，包含：

- 下载后镜像体积
- 定制后镜像体积
- 最终压缩后体积
- 输出文件名

---

## 🔧 自定义与修改

### 修改时区

编辑 `builder.sh` 第 68 行：

```bash
--timezone "Asia/Shanghai" \  # 改为你需要的时区
```

### 修改 Git 配置

编辑 `builder.sh` 第 143-144 行：

```bash
--run-command "HOME=/root git config --global user.name 'YourName'" \
--run-command "HOME=/root git config --global user.email 'your@email.com'" \
```

### 添加/移除软件包

编辑 `builder.sh` 第 75 行的 `--install` 参数，用逗号分隔包名。

### 修改 Docker 网络池

编辑 `builder.sh` 第 92-106 行的 `daemon.json` 配置。

### 禁用每日构建

删除或注释 `.github/workflows/daily-build.yml` 中的 `schedule` 部分。

---

## 📊 镜像体积参考

| 阶段     | 大小（参考） |
| -------- | ------------ |
| 官方下载 | ~500MB       |
| 定制后   | ~2.5GB       |
| 压缩后   | ~1.2GB       |

*实际大小因上游镜像和软件包版本变化而异*

---

## 🐛 常见问题 (FAQ)

<details>
<summary><b>Q: 为什么需要 root 权限运行？</b></summary>

`virt-customize` 和 `virt-sparsify` 需要直接访问镜像文件系统，需要 root 权限。在 GitHub Actions 中通过 `sudo -E` 运行。

</details>

<details>
<summary><b>Q: 能否在 macOS/Windows 上运行？</b></summary>

不建议。`libguestfs` 在非 Linux 系统上支持有限。推荐使用 Linux 虚拟机或 WSL2，或直接下载预构建镜像。

</details>

<details>
<summary><b>Q: 镜像默认密码是什么？</b></summary>

镜像基于官方云镜像，默认通过 cloud-init 注入 SSH 密钥，**没有预设密码**。需配合云平台（如 OpenStack、Proxmox、QEMU）的 cloud-init 使用。

</details>

<details>
<summary><b>Q: 如何在 QEMU/KVM 中使用镜像？</b></summary>

```bash
# 创建虚拟机
virt-install \
  --name debian13-custom \
  --memory 2048 \
  --vcpus 2 \
  --disk path=debian-13-generic-amd64-NEXT.qcow2 \
  --import \
  --os-variant debian11 \
  --network network=default \
  --graphics none \
  --console pty,target_type=serial
```

</details>

<details>
<summary><b>Q: 为什么构建时间这么长？</b></summary>

定制阶段需要：
1. 安装 60+ 个软件包
2. 编译安装 Xanmod 内核
3. 安装 Docker 及组件
4. 配置 Zsh + Zim Framework

大约需要 15-30 分钟，具体取决于网络速度和 CPU 性能。

</details>

<details>
<summary><b>Q: 如何更新已构建的镜像？</b></summary>

重新运行构建脚本即可。每次构建都会下载最新的上游镜像和软件包。建议定期更新（GitHub Actions 已配置每日构建）。

</details>

<details>
<summary><b>Q: 临时目录 `/root/.ImageMakerTemp` 是什么？</b></summary>

`virt-sparsify` 需要临时空间进行压缩操作。脚本会在退出时自动清理。在 GitHub Actions 中会使用 `$RUNNER_TEMP` 目录。

</details>

---

## 🚀 使用场景

- **云平台部署**：OpenStack、Proxmox VE、KVM 等
- **开发环境**：快速启动一个包含完整开发工具链的 VM
- **容器测试**：预装 Docker，适合容器化应用开发
- **性能测试**：Xanmod 内核 + BBR，适合网络性能测试
- **教学演示**：现代化 CLI 工具链，适合 Linux 教学

---

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

### 提交 PR 前请确保：

1. 代码通过 `shellcheck` 检查（如有）
2. 本地测试构建成功
3. 提交信息遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范（建议使用 gitmoji）

### 开发建议：

- 大型改动请先开 Issue 讨论
- 保持脚本的可读性和注释完整性
- 测试在不同 Linux 发行版上的兼容性

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE)

---

## 🙏 致谢

- [Debian](https://www.debian.org/) - 优秀的 Linux 发行版
- [Xanmod](https://xanmod.org/) - 高性能内核项目
- [Docker](https://www.docker.com/) - 容器化技术
- [Zim Framework](https://github.com/zimfw/zimfw) - 轻量级 Zsh 框架
- [Powerlevel10k](https://github.com/romkatv/powerlevel10k) - 强大的 Zsh 主题
- 所有现代化 CLI 工具的开发者们

---

## 📮 联系方式

- **Maintainer**: Lynricsy
- **Email**: im@ling.plus
- **GitHub**: [@Lynricsy](https://github.com/Lynricsy)

---

<div align="center">

**⭐ 如果这个项目对你有帮助，请给个 Star！**

Made with ❤️ by Lynricsy

</div>
