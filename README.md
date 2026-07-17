# Mineradio macOS

[Mineradio](https://github.com/XxHuberrr/Mineradio) 的 macOS 移植版 — 在保留原版所有核心功能的基础上适配 macOS 原生体验。

## 下载

前往 [Releases](../../releases) 下载 DMG：

| 文件 | 适用 |
|------|------|
| `Mineradio-1.1.1.dmg` | Intel Mac (x64) |
| `Mineradio-1.1.1-arm64.dmg` | Apple Silicon (M 系列) |

首次打开提示「未经验证的开发者」时，去 **系统设置 → 隐私与安全性** 点"仍要打开"。

## 与原版的差异

### 新增
- macOS 原生红绿灯信号灯（关闭 / 最小化 / 全屏）
- DMG 安装包（x64 + arm64 双架构）
- 谱面缓存路径使用 `~/Library/Caches/Mineradio/beatmaps`

### 不支持的 Windows 专有功能
- 桌面歌词 - 鼠标穿透（依赖 `user32.dll` + PowerShell 钩子）
- 窗口嵌入壁纸 WorkerW（依赖 Windows 桌面窗口机制）

以上功能在 macOS 上会被安全跳过，不影响主体功能。

## 开发运行

```bash
cd source
npm install
npm start
npm run build:mac:dir    # 构建目录包
npm run build:mac        # 构建 DMG
```

如果 Electron 二进制下载慢，设置镜像加速：

```bash
ELECTRON_MIRROR="https://npmmirror.com/mirrors/electron/" npm start
```

## 移植改动

- `desktop/main.js` — 平台常量、图标路径、GPU 开关、全屏机制
- `desktop/preload.js` — 暴露 `platform` 属性
- `server.js` — 谱面缓存路径适配
- `build/after-pack.js` — 跳过非 Windows 平台
- `build/icon.icns` — macOS 应用图标
- `package.json` — macOS 构建配置
- `public/index.html` — macOS 样式适配

## 授权

本项目是 [Mineradio](https://github.com/XxHuberrr/Mineradio) (Copyright © 2026 XxHuberrr) 的衍生作品，采用 **GNU General Public License v3.0**。
