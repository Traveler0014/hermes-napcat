# hermes-napcat 已安装 ✅

## 下一步

### 1. 部署 NapCat（QQ 消息中转）

推荐 Docker 部署：

```bash
docker run -d --name napcat --restart always \
    --network host \
    -e NAPCAT_GID=0 -e NAPCAT_UID=0 \
    -e WEBUI_PORT=6099 \
    -v /opt/napcat/QQ:/app/.config/QQ \
    -v /opt/napcat/config:/app/napcat/config \
    -v /opt/napcat/logs:/app/napcat/logs \
    mlikiowa/napcat-docker:latest
```

### 2. 登录与配置

浏览器打开 WebUI 扫码登录：

```bash
# 获取 WebUI token
docker logs napcat 2>&1 | grep "WebUi Token"
```

访问 `http://<服务器IP>:6099/webui?token=<token>`，手机 QQ 扫码登录。

在 WebUI「网络配置」中创建两个服务：

- **HTTP 服务器**：监听 `127.0.0.1:18801`，token 与 `access_token` 一致
- **WebSocket 客户端（反向 WS）**：URL `ws://127.0.0.1:18800`，token 相同

### 3. 启动 Hermes Gateway

```bash
hermes gateway install --system --run-as-user root
systemctl restart hermes-gateway
journalctl -u hermes-gateway -f
```

看到 `NapCat: reverse WS listening` 即成功。

### 4. 测试

QQ 私聊机器人即可测试。群聊需 @机器人。

---

完整文档：https://github.com/shubyi/hermes-napcat
管理命令：`hermes-napcat --help`
