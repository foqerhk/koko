# KoKo

通过 SSH 在 iPhone 上操作 Linux 服务器上的 **Cursor Agent CLI** 的原生终端 App。

## 文档

- [需求与技术方案](docs/native-mobile-agent-terminal-requirements.md)

## iOS 工程（Phase 1 MVP）

```bash
cd ios
chmod +x scripts/setup.sh
./scripts/setup.sh
open KoKo.xcodeproj
```

在 Xcode 中选择 iPhone 设备或模拟器运行。

### 已实现

- SSH 密钥生成（Ed25519 / ECDSA / RSA）与 Keychain 存储
- 主机配置、TOFU 主机指纹确认
- 会话列表（tmux / `agent persist`）
- Citadel SSH PTY + SwiftTerm 终端
- 快捷键栏（Esc、Tab、Ctrl+J、方向键等）
- Scrollback 本地全量保留
- 断线后 re-attach 同一 tmux 会话
- 检测 Cursor 登录 URL 并展示复制/打开
- iOS Simulator 编译通过（本地 SPM 依赖）

### 依赖

| 组件 | 许可证 |
|------|--------|
| SwiftTerm | MIT |
| Citadel | MIT |

详见 [ios/README.md](ios/README.md) 与 [ios/THIRD_PARTY_NOTICES.md](ios/THIRD_PARTY_NOTICES.md)。

## 验收链路

1. 在 App「密钥」页生成 Ed25519 密钥，公钥写入服务器 `~/.ssh/authorized_keys`
2. 「主机」页添加服务器与项目目录
3. 「会话」页创建 tmux 会话并进入终端
4. 远端执行 `agent` 交互；断开 App 后重新进入同一会话应 attach 原 tmux
