# Dowon Surge｜Sub-Store 使用

## 架构

Dowon 主配置不再内联 Sub-Store 的 Script/MITM。

职责分离：

```text
Dowon-Surge.conf
├─ DNS / Rule
├─ Entry / VPS Exit
├─ Korea VPS / USA VPS 的 policy-path
└─ [Host] sub.store = 127.0.0.1

Sub-Store 官方 Surge 模块
├─ MITM
├─ Sub-Store Core
├─ Sub-Store Simple
└─ 由 Sub-Store 官方维护更新
```

官方模块 URL：

```text
https://raw.githubusercontent.com/sub-store-org/Sub-Store/master/config/Surge.sgmodule
```

## 首次启用

1. 在 Surge 的模块页面安装并启用官方 Sub-Store 模块。
2. 在 Surge 中安装并信任 MITM 根证书。
3. 确保当前使用 Dowon Surge Profile，并在 Mac 测试时开启需要的网络接管。
4. 用 Safari 打开 `https://sub.store`。
5. 页面正常进入 Sub-Store 后，再添加 VPS。

主配置保留：

```ini
[Host]
sub.store = 127.0.0.1
```

这是 fail-safe：如果模块没有成功接管，`sub.store` 会在本机失败，不应访问到公网同名域名。

## 添加 Korea VPS

在 Sub-Store 新建本地“单订阅”：

```text
名称：Korea-VPS
内容：你的 Korea VPS 完整 ss:// URI
```

保存。

## 添加 USA VPS

同样创建：

```text
名称：USA-VPS
内容：你的 USA VPS 完整 ss:// URI
```

真实 `ss://` 含密码/密钥，不进入 GitHub，也不要发送到聊天里。

## Surge 如何读取

Dowon 主配置已经预接：

```text
https://sub.store/download/Korea-VPS/Surge
https://sub.store/download/USA-VPS/Surge
```

对应策略：

```text
Sub-Store 真实 VPS 可用
→ Korea VPS / USA VPS
→ underlying-proxy=Entry
→ 机场 → VPS → Internet

Sub-Store 未配置 / 输出为空 / VPS 不可用
→ Fail Closed
→ REJECT
```

不会因为 VPS 订阅为空而主动回退 DIRECT。

## 机场订阅

机场订阅目前仍由 Surge 原生 `policy-path` 本地填写：

```ini
我的节点 = select, Fail Closed, policy-path=你的订阅地址
```

真实机场 URL/Token 继续只保存在本地。以后若明确需要多机场，再考虑迁入 Sub-Store collection。
