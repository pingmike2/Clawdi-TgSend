# clawdi-send

定时给指定 Telegram 机器人对话窗口发消息（Telethon userbot）。主程序仓库，配套 workflow 在公开仓库 `Clawdi-TgSend`。

## 文件

- `tg_send.py` — 主程序：读取环境变量，用 StringSession 登录 TG，向目标 bot 对话发送一条含北京时间的消息
- `setup_tg_session.py` — 本机交互式生成 `TG_SESSION`（Telethon StringSession）

## 配置（公开仓库 Secrets / Variables）

| 名称 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `TG_API_ID` | Secret | ✅ | my.telegram.org → API development tools |
| `TG_API_HASH` | Secret | ✅ | 同上 |
| `TG_SESSION` | Secret | ✅ | 本机运行 `setup_tg_session.py` 生成 |
| `PRIVATE_REPO_TOKEN` | Secret | ✅ | PAT（scope: repo），用于 workflow 检出本私库 |
| `TG_TARGET` | Variable | ❌ | 目标 bot 用户名，默认 `ClawdiAgent1Bot` |
| `TG_MSG_TEXT` | Variable | ❌ | 消息模板，`{time}` 占位符替换为北京时间，默认 `纯测试，现在北京时间 {time}` |
| `TG_TIME_FMT` | Variable | ❌ | 北京时间格式，默认 `%Y-%m-%d %H:%M:%S` |
| `TG_PROXY` | Secret | ❌ | Telethon 代理，如 `socks5://host:1080` 或 `socks5://user:pass@host:1080`、`http://host:port`；不填直连 |
| `REPORT_BOT_TOKEN` | Secret | ❌ | 报告推送用的 Bot Token（与发消息账号分离），不填则不推报告 |
| `REPORT_CHAT_ID` | Secret | ❌ | 报告接收的 chat id，与上项配套 |

## 本机生成 TG_SESSION

```bash
pip3 install telethon
TG_API_ID=123456 TG_API_HASH=abcdef... python3 setup_tg_session.py
```

按提示输入手机号 → 验证码（→ 两步验证密码），输出的一串字符即 `TG_SESSION`。

## 本机测试（可选）

```bash
export TG_API_ID=... TG_API_HASH=... TG_SESSION=...
python3 tg_send.py
```
