介绍：

利用 Caddy 支持 SNI 分流特性，对 VLESS+Vision+REALITY、Trojan+RAW+TLS、HTTP/3 server 进行 SNI 分流（四层转发），实现除 Xray 的 mKCP 应用外各应用共用 443 端口。其中 Caddy 同时为 VLESS+Vision+REALITY 与 Trojan+RAW+TLS 提供 WEB 服务（转发与回落应用），为 Xray 的 WebSocket、H2C、gRPC 提供反向代理，为 forwardproxy 插件提供正向代理，其应用如下：

1、M=VLESS+Vision+REALITY（转发配置，REALITY 所需 TLS 由 Caddy 提供。）

2、F=Trojan+RAW+TLS（回落/分流配置，TLS 由自己启用及处理。）

3、B=VMess+WebSocket+TLS（TLS 由 Caddy 提供及处理，不需配置。）

4、D=VLESS+H2C+TLS（TLS 由 Trojan+RAW+TLS 提供及处理，不需配置。）

5、G=Shadowsocks+gRPC+TLS（TLS 由 Caddy 提供及处理，不需配置。）

6、A=VLESS+mKCP+seed

7、N=NaiveProxy（基于 Caddy 的改进版 forwardproxy 插件实现，TLS 由 Caddy 提供及处理。）

注意：

1、Caddy 加 caddy-l4 插件定制编译才可以实现 SNI 分流及定向 UDP 转发。

2、Xray 版本不小于 v1.8.0 才支持 REALITY。

3、Xray 的监听地址不支持 Shadowsocks 协议使用 UDS 监听。

4、Xray 的 Shadowsocks 2022 新协议提升了性能并带有完整的重放保护。

5、Caddy 支持 H2C server 与 HTTP/1.1 server 共用一个端口或一个进程。

6、Caddy 版本不小于 v2.7.0 才默认支持 PROXY protocol 接收。若 Caddy 版本小于 v2.7.0 需加 caddy2-proxyprotocol 插件定制编译才支持 PROXY protocol 接收。

7、Caddy 版本不小于 v2.6.0 才支持 H2C/gRPC 代理的 UDS 转发。

8、使用本人 Releases 中编译好的 Caddy 文件，可同时支持 SNI 分流及定向 UDP 转发、NaiveProxy 及 PROXY protocol 等应用。

9、本示例 NaiveProxy 除了支持 HTTPS 协议连接，还同时支持 QUIC 协议连接，即支持 HTTP/3 正向代理应用。Caddy 启用 HTTP/3 支持，建议[增加服务端系统的 UDP 缓冲区大小](https://github.com/quic-go/quic-go/wiki/UDP-Buffer-Sizes)。

10、本示例所需 TLS 证书由 Caddy（内置 ACME 客户端） 提供，实现 TLS 证书自动申请及更新。

11、配置1：使用 Local Loopback 连接，且启用了 PROXY protocol。配置2：使用 UDS 连接（对应 HTTP/3 server、Shadowsocks+gRPC+TLS 除外），且启用了 PROXY protocol。

12、本示例 F 兼容原版 Trojan 的服务端应用，即可使用 Trojan 或 Trojan-Go 客户端连接。
