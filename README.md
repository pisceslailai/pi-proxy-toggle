# pi-proxy-toggle

Pi 扩展：快速切换 HTTP 代理开关，并在状态栏显示当前代理状态。

## 安装

```bash
pi install git:github.com/pisceslailai/pi-proxy-toggle
```

安装后 `/reload` 或重启 pi 生效。

## 命令

| 命令 | 作用 |
|---|---|
| `/proxy on` | 启用代理（写入环境变量 + 自动 reload 生效） |
| `/proxy off` | 禁用代理（清除环境变量 + 自动 reload 生效） |
| `/proxy status` | 查看当前状态 |
| `/proxy set <url>` | 修改代理地址（默认 `http://127.0.0.1:7890`） |
| `/proxy statusbar on\|off` | 控制状态栏是否显示代理状态 |

## 状态栏

启用后 footer 状态栏实时显示：

- `代理:开 http://127.0.0.1:7897` — 代理开启（含地址）
- `代理:关` — 代理关闭

## 工作原理

- pi 使用 undici 的 `EnvHttpProxyAgent` 代理出站请求，该 agent 在**构造时**读取 `HTTP_PROXY` / `HTTPS_PROXY` 环境变量
- 命令修改 `process.env` 后自动触发 `/reload`（pi 的 reload 流程会重建 HTTP dispatcher），无需重启
- 开关状态持久化在 `~/.pi/agent/proxy-toggle.json`，下次启动 pi 自动应用
- 若 shell 已手动设置代理环境变量，扩展不会覆盖

## 注意

- 代理开启状态下若代理软件退出，所有网络请求（含 deepseek 等国内 API）都会失败，执行 `/proxy off` 恢复直连
- 建议代理软件使用规则分流模式（国内直连、国外走代理）

## License

MIT
