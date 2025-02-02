介绍：

本示例使用 Caddy 源码加改进版 forwardproxy 插件、caddy-trojan 插件编译而成 Caddy 文件，实现了 NaiveProxy 与 Trojan-Go 服务端加伪装网站应用。

注意：

1、使用本人 Releases 中编译好的 Caddy 文件，可同时支持 NaiveProxy、Trojan-Go 等应用。

2、本示例 NaiveProxy 除了支持 HTTPS 协议连接，还同时支持 QUIC 协议连接，即支持 HTTP/3 正向代理应用。Caddy 启用 HTTP/3 支持，建议[增加服务端系统的 UDP 缓冲区大小](https://github.com/quic-go/quic-go/wiki/UDP-Buffer-Sizes)。

3、本示例 Trojan-Go 仅兼容原版服务端的核心应用：支持 Trojan 应用与 Trojan-Go 的 WebSocket 应用共存、支持 CDN 流量中转(基于 WebSocket over TLS)，其 WebSocket 应用不支持 WebSocket 0-RTT 与多路复用。客户端推荐选择 Xray 客户端，支持使用指纹伪造。

4、本示例 Caddy 支持自动 HTTPS，即自动申请与更新 TLS 证书，自动 HTTP 重定向到 HTTPS。
