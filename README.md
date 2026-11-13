# yank 懒猫微服打包

上游：https://github.com/ca-x/yank （Apache-2.0 / MIT）

自托管剪贴板同步服务：跨设备端到端加密同步剪贴板历史，可选服务端存储。

## 打包说明

- 镜像：`ghcr.io/ca-x/yank`（semver tag 如 0.2.0，amd64/arm64），由 [ca-x/lazycat-github-action](https://github.com/ca-x/lazycat-github-action) 镜像模式自动同步，定时 + 手动触发发布喵喵商店
- API/Web（7219）走 upstreams（懒猫可信 HTTPS 反代）
- **中继（7220）走 TCP ingress 直通**：iroh-relay 明文 HTTP 协议，`YANK_RELAY_URL=http://yank.<域名>:7220`（客户端解析为 iroh RelayUrl，接受 http scheme）
- NAT 地址发现（QUIC 7842/udp）未发布：无证书场景下不可用，剪贴板同步不依赖它
- 管理员账号/密码为设置向导参数（密码随机默认值，首次启动必需）
- 数据：`/lzcapp/var/data` → `/data`（SQLite 账号库 + 用户存储目录）
- Cookie 安全：`YANK_COOKIE_SECURE=true`（配合懒猫 HTTPS）

## 使用

1. 打开应用，用部署时设置的管理员账号登录（管理台支持创建成员账号）
2. 桌面/手机 yank 客户端：服务地址 `https://yank.<懒猫域名>`，中继地址 `http://yank.<懒猫域名>:7220`
3. 剪贴板内容端到端加密；服务端存储为可选功能
