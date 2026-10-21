# Dowon Surge

Dowon Surge 是一套基于成熟 Surge 配置裁剪并加入个人路由逻辑的配置。

## 当前状态

**Public Beta**

当前 Beta 已包含：
- Korea / Hong Kong / Singapore / Japan / USA / Taiwan 六地区自动组；
- 结构性 `Fail Closed = reject`，用于阻止空组触发 Surge `SUBSTITUTE(DIRECT)`；
- `其他节点` 手动组；
- `Entry`；
- Sub-Store 内联支持；
- `Korea VPS` / `USA VPS` 从本机 Sub-Store 读取 `ss://` 并转换为 Surge；
- `VPS Exit`；
- `Entry → VPS` 链式代理；
- `Video Exit`；
- 中国明确白名单入口；
- 视频规则入口；
- `FINAL → VPS Exit`。

Mac Beta 已实际发现 Surge 在策略组无可用成员时会使用 `SUBSTITUTE`（DIRECT 别名），因此当前版本已针对这一行为修正 fail-closed 结构。

## 文件

- `config/Dowon-Surge.conf` — 当前 Public Beta 配置
- `rules/china-direct.list` — 中国明确直连白名单
- `rules/video.list` — 用户指定视频服务规则
- `docs/SUBSTORE_SETUP.md` — Sub-Store 与 VPS 添加说明

## 机场订阅

机场订阅仍只在 Surge 本地填写：

```ini
我的节点 = select, Fail Closed, policy-path=你的订阅地址, ...
```

真实订阅 URL/Token 不进入 GitHub。

## VPS：改用 Sub-Store

不需要再在 Surge 里手填 SS 的 IP、端口、加密方式和密码。

更新配置后：
1. 在 Surge 中安装并信任 MITM 根证书；
2. Safari 打开 `https://sub.store`；
3. 新建本地单订阅 `Korea-VPS`，粘贴韩国 VPS 的完整 `ss://`；
4. 新建本地单订阅 `USA-VPS`，粘贴美国 VPS 的完整 `ss://`；
5. Surge 会通过本机 Sub-Store 输出自动生成可用的 Surge 节点。

主配置读取：

```text
https://sub.store/download/Korea-VPS/Surge
https://sub.store/download/USA-VPS/Surge
```

配置还包含：

```ini
[Host]
sub.store = 127.0.0.1
```

这是安全保护：如果 Sub-Store 脚本没有接管，请求留在本机失败，不把本地订阅数据误发到公网 `sub.store`。

## Fail-Closed

地区自动组现在使用 `url-test + Fail Closed + evaluate-before-use=true`。这是因为 Mac Beta 已实测空组会 DIRECT，不再继续沿用原来的空 `smart` 风险。

VPS 组使用：

```text
真实 Sub-Store VPS 可用 → Entry → VPS
未配置 / 为空 / VPS 不可用 → Fail Closed → REJECT
```

不主动添加 DIRECT、跨地区 fallback 或跨 VPS fallback。

## 当前规则行为

```text
LAN                  → DIRECT
china-direct.list    → DIRECT
video.list           → Video Exit
其他流量             → VPS Exit → selected VPS (via Entry)
```

`china-direct.list` 和 `video.list` 当前仍只包含说明注释，没有预置真实业务域名。

## Beta 提醒

这仍然是 Beta。下一步需要在 Mac/iPhone Surge 实测：
- Sub-Store 页面是否正常；
- `Korea-VPS` / `USA-VPS` 是否能从 ss:// 正确输出；
- Entry → VPS 链式实际出口；
- 机场订阅失效或地区零匹配时是否保持 REJECT 而不是 DIRECT；
- DNS / UDP；
- Video / China Direct。
