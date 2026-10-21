# Dowon Surge

Dowon Surge 是一套基于成熟 Surge 配置裁剪并加入个人路由逻辑的配置。

## 当前状态

**Public Beta**

当前 Beta 已包含：
- Korea / Hong Kong / Singapore / Japan / USA / Taiwan 六地区 `smart` 组；
- `其他节点` 手动组；
- `Entry`；
- `Korea VPS` / `USA VPS` fail-closed 占位；
- `VPS Exit`；
- `Entry → VPS` 链式代理；
- `Video Exit`；
- 中国明确白名单入口；
- 视频规则入口；
- `FINAL → VPS Exit`。

当前尚未包含正式 Sukka 广告主干和 Harrison 七源 Supplement；这些会在 Core Beta 验证后继续加入。

## 文件

- `config/Dowon-Surge.conf` — 当前 Public Beta 配置
- `rules/china-direct.list` — 中国明确直连白名单
- `rules/video.list` — 用户指定视频服务规则

## 导入前 / 导入后需要本地填写

本仓库不保存私人凭据。

配置中的：

```ini
我的节点 = select, policy-path=你的订阅地址, ...
```

需要由使用者在 Surge 本地替换成自己的机场订阅地址。

两个 VPS 当前故意使用：

```ini
Korea VPS = reject
USA VPS = reject
```

使用者需要在本地替换为自己的真实 Surge 代理定义。

在未填写真实 VPS 前，`FINAL → VPS Exit` 会失败而不是回退到 DIRECT。这是有意的 fail-closed 设计。

## 当前规则行为

```text
LAN                  → DIRECT
china-direct.list    → DIRECT
video.list           → Video Exit
其他流量             → VPS Exit → Entry → VPS
```

`china-direct.list` 和 `video.list` 当前只包含说明注释，还没有猜测或预置真实业务域名。

## Beta 提醒

这是实机验证阶段，不是 Stable。

需要重点验证：
- 机场订阅加载；
- 六地区 `smart`；
- Entry 切换；
- Korea / USA VPS；
- Entry → VPS 链式；
- Video Exit；
- China Direct；
- DNS / UDP；
- 入口或 VPS 故障时是否保持 fail-closed。

发现实际问题后再针对问题修改。
