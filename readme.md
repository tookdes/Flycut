# Flycut 维护版

Flycut 是一款轻量的 macOS 剪贴板管理工具，基于开源项目 Jumpcut。它会记录文本剪贴板历史，并允许你通过全局快捷键在历史项之间切换、粘贴常用内容。

本仓库是 [TermiT/Flycut](https://github.com/TermiT/Flycut) 的维护分支，用于整理近年 macOS 兼容性、中文本地化和无 App Store 分发版本的修复。原项目、贡献者和 MIT 许可证仍然保留。

## 下载与安装

从 [tookdes/Flycut Releases](https://github.com/tookdes/Flycut/releases) 下载最新的 `Flycut-*-Release.zip`，解压后把 `Flycut.app` 放到 `/Applications`。

首次运行后，请在系统设置中授予辅助功能权限：

`系统设置` -> `隐私与安全性` -> `辅助功能` -> 启用 `Flycut`

当前维护版是未签名的 DRM-Free 构建。如果 macOS 阻止首次打开，可以在 Finder 中右键点击 `Flycut.app`，选择“打开”，再确认运行。

## 本次维护更新

### 1.9.7 维护版

- 完成简体中文本地化，覆盖主菜单、偏好设置、弹窗、剪贴板浮层、Info.plist 和 ShortcutRecorder 提示文本。
- 增加英文 string table 作为 `NSLocalizedString` 基线，避免后续继续把 UI 文案硬编码在源码里。
- 替换应用图标，使菜单栏和应用图标更适配新版本 macOS。
- 将 macOS 最低部署目标调整为 10.13，适配当前 Xcode 构建环境。
- 修复沙盒状态检测，不再依赖路径层级猜测，改为读取进程 entitlement。
- 更新“登录时启动”逻辑：macOS 13 及以上使用 `SMAppService`，旧系统继续使用原有登录项注册方式。
- 改善无 iCloud entitlement 构建下的 CloudKit 行为，避免开启同步时崩溃，并在缺少容器配置时安全跳过。
- 修复若干快捷键和粘贴边界问题，包括空按键事件保护、仅在浮层显示时响应修饰键释放粘贴、以及使用稳定的 Cmd-V key code。
- 保留本地 Release zip 作为 GitHub Release 资产，不再把编译产物提交进 git。

## 使用

默认全局快捷键是 `Shift-Command-V`。按下快捷键后，Flycut 会显示最近的剪贴板历史；继续按快捷键或方向键切换条目，释放修饰键后粘贴当前条目。快捷键、历史数量、菜单栏图标、浮层样式和 iCloud 同步可在偏好设置中调整。

更多原始说明可参考：

- [Mac 帮助](help.md)
- [iOS 帮助](help.iOS.md)

## 从源码构建

需要 macOS、Xcode 和命令行工具。

```sh
xcodebuild -project Flycut.xcodeproj -scheme Flycut -configuration Release build CODE_SIGNING_ALLOWED=NO
```

Release 构建完成后，`.app` 会出现在 Xcode 的 DerivedData 产品目录中。可以使用下面的方式生成可上传的 zip：

```sh
APP=/path/to/DerivedData/Products/Release/Flycut.app
ditto -c -k --keepParent "$APP" "Flycut-1.9.7-zh-Hans-Release.zip"
```

本仓库会忽略 `.app`、zip、DerivedData、归档、dSYM 等本地构建产物；发布包请通过 GitHub Release 上传。

## 上游与许可证

- 上游项目：[TermiT/Flycut](https://github.com/TermiT/Flycut)
- 本维护分支：[tookdes/Flycut](https://github.com/tookdes/Flycut)
- 许可证：MIT

如果你喜欢 Flycut，也可以参考原项目的捐赠链接：[paypal.me/flycut](https://paypal.me/flycut)
