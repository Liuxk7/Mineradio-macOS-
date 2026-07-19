# Mineradio macOS v1.1.1.2

[Mineradio](https://github.com/XxHuberrr/Mineradio) 的 macOS 移植版 — 在 v1.1.1.1 基础上修复媒体键、控制中心、全屏体验和滚动手感。

## 下载

前往 [Releases](../../releases) 下载 DMG：

| 文件 | 适用 |
|------|------|
| `Mineradio-1.1.1.2.dmg` | Intel Mac (x64) |
| `Mineradio-1.1.1.2-arm64.dmg` | Apple Silicon (M 系列) |

首次打开提示「未经验证的开发者」时，去 **系统设置 → 隐私与安全性** 点"仍要打开"。

## 相对 v1.1.1.1 的改动

### 修复
- **MacBook 媒体键切歌** — `MediaPlayPause` / `MediaNextTrack` / `MediaPreviousTrack` 全局快捷键，F7/F8/F9 可播放/暂停/下一首/上一首
- **控制中心 Now Playing** — 菜单栏播放控件显示专辑封面、歌名，支持切歌和播放/暂停
- **绿灯真全屏** — 恢复原生 `setFullScreen`，点击绿灯进入新 Space 并隐藏菜单栏，不再仅填充屏幕
- **窗口重新打开** — 红点关闭窗口改为 hide 而非 destroy，再打开时直接显示已运行的窗口，server 不中断
- **桌面歌词** — macOS 上默认 `clickThrough=false`，避免窗口创建后锁死在点击穿透模式无法交互
- **触摸板滚动** — 左侧歌单列表滚动速度 dampen 至 0.32x；右侧 3D 歌单架改为增量累计式滚动，慢划慢滚、快划快滚

### 新增
- W3C Media Session API — `navigator.mediaSession.metadata` 设封面/歌名/歌手，`playbackState` 同步播放/暂停
- `platform-darwin` CSS class — macOS 特有样式隔离，隐藏自定义窗口按钮、移除不透明窗口圆角
- `close` 事件 `preventDefault` + `hide` — 标准 macOS 窗口行为

### 移除
- `setSimpleFullScreen` / `isSimpleFullScreen` 手动全屏 workaround（窗口已不透明，原生 `setFullScreen` 正常）
- `maximize` 事件重定向全屏的逻辑，绿灯直通原生 `setFullScreen`
- 全屏时动态切换 `backgroundColor` 的 hack

## 不支持的 Windows 专有功能

- 桌面歌词鼠标穿透切换（依赖 `user32.dll` + PowerShell 钩子）
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

- `desktop/main.js` — `setFullScreen` 恢复、`registerMacOSMediaKeys`、`close` 事件 hide、桌面歌词 clickThrough 默认值
- `desktop/preload.js` — 暴露 `platform` 属性、注入 `platform-darwin` class
- `public/index.html` — macOS CSS 样式、Media Session API、触摸板滚动 dampen、3D 歌单架增量累计滚动
