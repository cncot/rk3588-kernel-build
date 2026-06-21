# RK3588 内核编译（rknpu v0.9.8）

为 OrangePi 5 Plus (RK3588) 编译自定义内核，将 rknpu 驱动从 built-in 改为可加载模块，并升级到 v0.9.8。

## 文件结构

```
├── kernel_config                              # Armbian 6.1.75-vendor-rk35xx 内核配置（CONFIG_ROCKCHIP_RKNPU=m）
├── .github/workflows/build-kernel.yml         # GitHub Actions 工作流
```

## 触发方式

- **手动触发**: GitHub → Actions → "Build RK3588 Kernel" → Run workflow
- **Push 触发**: 推送到 main 分支

## 产物

工作流运行后会产出 artifacts（.deb 包 + kernel Image + rknpu.ko）：

1. **linux-image-*.deb** — 内核镜像包（含 /boot/Image + DTBs）
2. **linux-headers-*.deb** — 内核头文件包（用于编译外部模块）
3. **linux-modules-*.deb** — 内核模块包（含 rknpu.ko）
4. **rknpu.ko** — 独立的 rknpu 驱动模块 v0.9.8

## 安装到 OrangePi

下载 artifacts 后传到 OrangePi：

```bash
# 解压 artifacts
tar xzf rk3588-kernel-rknpu-v0.9.8.tar.gz

# 安装 .deb 包（全部）
sudo dpkg -i linux-*.deb

# 或只安装 rknpu 模块（如果不想换内核）
# 注意：需要把旧驱动从 built-in 改成模块，否则需要重启
```

## 注意

- 内核基于 Rockchip `develop-6.1`（commit `b4ef083dc`）
- 内核配置取自 Armbian 26.5.1 (6.1.75-vendor-rk35xx)
- 交叉编译使用 `gcc-aarch64-linux-gnu` (Ubuntu 24.04)
- rknpu 驱动版本: **0.9.8**（原 Armbian 内置为 0.9.7）
